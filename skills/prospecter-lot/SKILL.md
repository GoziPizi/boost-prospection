---
description: Prospecte une liste d'entreprises à partir d'un fichier CSV ou Excel (par exemple un export Pappers), avec un fichier de suivi qui permet de reprendre après une interruption. Pour quelques noms tapés à la main, utiliser prospecter.
disable-model-invocation: true
---

# Prospecter une liste

Le fichier à traiter : $ARGUMENTS

Le fichier peut être un CSV ou un fichier Excel (.xlsx, .xls). Si aucun fichier n'est donné, demande à l'utilisateur de le glisser dans la conversation.

## Avant de commencer

1. Tu cherches le fichier `config-boost-prospection.md` à la racine du dossier de travail (le dossier ouvert pour cette session).
   - Aucun dossier de travail ouvert : tu demandes à l'utilisateur d'ouvrir son dossier de prospection, et tu t'arrêtes là.
   - Le fichier n'existe pas : tu lances le skill demarrer, puis tu reprends ici.
2. Tu lis les règles d'ajout `${CLAUDE_PLUGIN_ROOT}/regles/ajout-hubspot.md`.

## Étape 1 : lire le fichier

Tu repères les colonnes utiles d'après leur titre :

- **le nom de l'entreprise** (obligatoire) : « Dénomination », « Raison sociale », « Nom », « Entreprise »…
- et si elles existent : SIREN, ville, site internet, nom du dirigeant.

Si tu ne trouves pas de colonne de nom, tu montres les titres des colonnes à l'utilisateur et tu lui demandes laquelle utiliser.

Une entreprise est identifiée par son SIREN s'il y en a un, sinon par son nom.

## Étape 2 : le fichier de suivi

Le suivi est toujours un fichier CSV, rangé à la racine du dossier de travail (jamais à côté du fichier déposé, qui peut disparaître à la fin de la conversation). Il porte le nom du fichier d'entrée suivi de `_suivi.csv` : par exemple `export-pappers_suivi.csv` pour `export-pappers.xlsx`.

- **Il n'existe pas** : tu le crées, encodé en UTF-8 avec BOM, avec le séparateur « ; » et cette ligne d'en-tête :

  `SIREN;Entreprise;Statut;Dirigeant 1;Fonction 1;Email 1;Dirigeant 2;Fonction 2;Email 2;Email général;Standard;Lien HubSpot;Raison;Date`

  Les colonnes « Dirigeant » contiennent le prénom et le nom. Si l'entreprise n'a qu'un dirigeant, les colonnes du dirigeant 2 restent vides.

- **Il existe** : tu le lis. Les entreprises au statut « ajouté » ou « arrêté » sont déjà traitées : tu les sautes. Celles au statut « erreur » sont retraitées. Si une entreprise apparaît plusieurs fois, seule la dernière ligne compte.

## Étape 3 : annoncer et démarrer

Tu annonces en quelques lignes :

- le nombre d'entreprises à traiter, et celles déjà traitées s'il y en a
- le temps estimé (environ 1 minute par entreprise, divisé par 3 grâce au traitement en parallèle)
- si Apollo est activé : jusqu'à 2 crédits Apollo par entreprise (un par dirigeant sans email sur le site)

Puis, si ce n'est pas déjà fait dans cette session, tu demandes une seule fois avec l'outil AskUserQuestion : « Je crée les transactions et les contacts dans HubSpot sans te demander de confirmation pour cette session ? ». Tu ne reposes jamais cette question pendant la session.

## Étape 4 : traiter la liste

Tu traites les entreprises par groupes de 3 :

1. Avec l'outil Agent, tu lances 3 sous-agents `boost-prospection:chercheur-prospect` en même temps (trois appels dans un seul message), un par entreprise, avec son nom, les infos déjà connues du fichier (SIREN, ville, site, dirigeant), les infos à trouver, les critères de sélection et le réglage Apollo de la config.
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
