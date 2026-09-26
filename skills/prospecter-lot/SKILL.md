---
description: Prospecte une liste d'entreprises à partir d'un fichier CSV (par exemple un export Pappers), avec un fichier de suivi qui permet de reprendre après une interruption. Pour quelques noms tapés à la main, utiliser prospecter.
disable-model-invocation: true
---

# Prospecter une liste

Le fichier à traiter : $ARGUMENTS

Si aucun fichier CSV n'est donné, demande à l'utilisateur de le glisser dans la conversation.

## Avant de commencer

1. Tu lis `${CLAUDE_PLUGIN_DATA}/config.md`. Si elle n'existe pas, tu lances le skill demarrer, puis tu reprends ici.
2. Tu lis les règles d'ajout `${CLAUDE_PLUGIN_ROOT}/regles/ajout-hubspot.md`.

## Étape 1 : lire le fichier

Tu repères les colonnes utiles d'après leur titre :

- **le nom de l'entreprise** (obligatoire) : « Dénomination », « Raison sociale », « Nom », « Entreprise »…
- et si elles existent : SIREN, ville, site internet, nom du dirigeant.

Si tu ne trouves pas de colonne de nom, tu montres les titres des colonnes à l'utilisateur et tu lui demandes laquelle utiliser.

Une entreprise est identifiée par son SIREN s'il y en a un, sinon par son nom.

## Étape 2 : le fichier de suivi

Le suivi s'appelle comme le fichier d'entrée, avec `_suivi` avant l'extension (par exemple `export-pappers_suivi.csv`), dans le même dossier.

- **Il n'existe pas** : tu le crées, encodé en UTF-8 avec BOM, avec le séparateur « ; » et cette ligne d'en-tête :

  `SIREN;Entreprise;Statut;Prénom;Nom;Email;Email général;Téléphone;LinkedIn;Lien HubSpot;Raison;Date`

- **Il existe** : tu le lis. Les entreprises au statut « ajouté » ou « arrêté » sont déjà traitées : tu les sautes. Celles au statut « erreur » sont retraitées. Si une entreprise apparaît plusieurs fois, seule la dernière ligne compte.

## Étape 3 : annoncer et démarrer

Tu annonces en quelques lignes :

- le nombre d'entreprises à traiter, et celles déjà traitées s'il y en a
- le temps estimé (environ 1 minute par entreprise, divisé par 3 grâce au traitement en parallèle)
- si Apollo est activé : jusqu'à 1 crédit Apollo par entreprise sans email sur son site

Puis, si ce n'est pas déjà fait dans cette session, tu demandes une seule fois avec l'outil AskUserQuestion : « Je crée les transactions et les contacts dans HubSpot sans te demander de confirmation pour cette session ? ». Tu ne reposes jamais cette question pendant la session.

## Étape 4 : traiter la liste

Tu traites les entreprises par groupes de 3 :

1. Avec l'outil Agent, tu lances 3 sous-agents `boost-prospection:chercheur-prospect` en même temps (trois appels dans un seul message), un par entreprise, avec son nom et les infos déjà connues du fichier (SIREN, ville, site, dirigeant).
2. Quand les 3 fiches sont revenues, tu les traites une par une :
   - **TROUVÉ** : tu l'ajoutes dans HubSpot en suivant les règles d'ajout. Statut « ajouté ».
   - **ARRÊTÉ** : statut « arrêté », avec la raison.
   - **Problème** (sous-agent en échec, erreur HubSpot…) : statut « erreur », avec la raison.
3. Après chaque entreprise, tu ajoutes sa ligne à la fin du fichier de suivi, sans réécrire les autres lignes. Tu remplis la date du jour, et tu mets les valeurs qui contiennent un « ; » entre guillemets.

Tu ne fais aucune recherche toi-même : tout passe par les sous-agents.

Toutes les 20 entreprises, tu fais un point en une ligne : « 40/200 traitées : 28 ajoutées, 11 arrêtées, 1 erreur ».

Si une entreprise pose problème, tu notes l'erreur et tu continues. Tu ne t'arrêtes jamais pour tout le lot.

## Le récapitulatif final

```
Terminé : X entreprises traitées
- Ajoutées : X
- Arrêtées : X (principales raisons : …)
- Erreurs : X (relance la même commande pour les retraiter)
- Crédits Apollo utilisés : X
Suivi complet : [chemin du fichier de suivi]
```
