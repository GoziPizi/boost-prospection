---
description: Prospecte une liste d'entreprises de bout en bout, de la recherche des coordonnées jusqu'à l'ajout dans HubSpot et au brouillon de message.
disable-model-invocation: true
---

# Prospecter

Les entreprises à prospecter : $ARGUMENTS

Chaque entreprise peut être donnée par son URL ou par son nom. Si la liste est vide, demande-la à l'utilisateur.

## Ton rôle

Tu es le chef d'orchestre : tu ne fais aucune recherche toi-même. Tu appelles les autres skills dans l'ordre, tu leur passes la fiche prospect et tu appliques les règles de la config.

## Étape 0 : la config

Tu lis le fichier `${CLAUDE_PLUGIN_DATA}/config.md`.

S'il n'existe pas, tu lances le skill demarrer. Une fois la config créée, tu reprends au début de ce skill.

Tu retiens dans la config :

- les étapes activées (section de chaque étape, ligne « Activée »)
- les règles d'arrêt
- la validation avant HubSpot (oui / non)
- le mode silencieux (oui / non)

## La fiche prospect

Pour chaque entreprise, tu tiens une fiche avec ces champs. Chaque skill la reçoit, la complète et te la rend.

```
Entreprise :
Site :
Page de contact :
Standard :
Dirigeant (prénom, nom, fonction) :
Email :
Téléphone :
LinkedIn entreprise :
LinkedIn dirigeant :
Score :
Statut : en cours | ajouté | écarté | erreur
Raison :
Liens HubSpot :
```

Un champ inconnu reste vide. Tu n'inventes jamais une valeur.

## Les étapes, pour chaque entreprise

Tu traites les entreprises une par une, dans cet ordre. Tu sautes toute étape désactivée dans la config.

1. **recherche-web**, seulement si tu n'as pas l'URL du site ou si recherche-linkedin est activée. Si le verdict est INCERTAIN ou INTROUVABLE : statut écarté, raison = le verdict (avec les candidats s'il y en a), entreprise suivante.
2. **verification-doublon-hubspot**, avec le nom et le site. Si le verdict est TRANSACTION EXISTANTE : statut écarté, raison « déjà dans HubSpot », entreprise suivante.
3. **recherche-site** : trouve le site si tu n'as que le nom, puis les coordonnées.
4. **recherche-linkedin**, avec le nom du dirigeant et de l'entreprise. Si le verdict est TROUVÉ, tu reportes l'URL dans « LinkedIn dirigeant ». Quel que soit le verdict, tu continues.
5. **enrichissement-apollo**
6. **scoring**
7. Tu appliques les **règles d'arrêt** de la config. Si une règle n'est pas respectée : statut écarté, raison = la règle, entreprise suivante.
8. Si la **validation avant HubSpot** est activée : tu montres la fiche à l'utilisateur et tu attends son accord. S'il refuse : statut écarté, raison « refusé par l'utilisateur ».
9. **ajout-hubspot** : statut ajouté.
10. **redaction-brouillon**

## Si une étape échoue

Tu notes l'erreur dans la fiche (statut erreur, raison en quelques mots) et tu passes à l'entreprise suivante. Tu ne t'arrêtes jamais pour tout le lot.

## Ce que tu écris pendant le traitement

- Mode silencieux activé : rien, jusqu'au récapitulatif.
- Sinon : une ligne par entreprise terminée, par exemple « Dupont SAS : ajouté (score 72) ».

## Le récapitulatif final

```
Ajoutés (X) :
- Entreprise : contact, score, lien HubSpot

Écartés (X) :
- Entreprise : raison

Erreurs (X) :
- Entreprise : raison
```

Tu n'affiches une rubrique que si elle contient au moins une entreprise.