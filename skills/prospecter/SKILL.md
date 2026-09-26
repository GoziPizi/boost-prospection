---
description: Prospecte une ou quelques entreprises données par leur nom - trouve le dirigeant et ses coordonnées, puis crée la transaction et le contact dans HubSpot. Pour une liste en fichier CSV, utiliser prospecter-lot.
disable-model-invocation: true
---

# Prospecter

Les entreprises à prospecter : $ARGUMENTS

Si la liste est vide, demande-la à l'utilisateur. Si l'utilisateur donne un fichier CSV, dis-lui d'utiliser plutôt `/boost-prospection:prospecter-lot`.

## Avant de commencer

1. Tu lis `${CLAUDE_PLUGIN_DATA}/config.md`. Si elle n'existe pas, tu lances le skill demarrer, puis tu reprends ici.
2. Tu lis les règles d'ajout `${CLAUDE_PLUGIN_ROOT}/regles/ajout-hubspot.md`.
3. Si ce n'est pas déjà fait dans cette session, tu demandes une seule fois avec l'outil AskUserQuestion : « Je crée les transactions et les contacts dans HubSpot sans te demander de confirmation pour cette session ? ». Tu ne reposes jamais cette question pendant la session.

## Pour chaque entreprise

Tu traites les entreprises une par une.

1. Tu lances le sous-agent chercheur-prospect avec le nom de l'entreprise. Tu ne fais aucune recherche toi-même.
2. Fiche ARRÊTÉ : tu notes la raison et tu passes à l'entreprise suivante.
3. Fiche TROUVÉ : tu l'ajoutes dans HubSpot en suivant les règles d'ajout, puis tu écris : « Nouvelle transaction créée : [entreprise], avec le contact : [prénom nom] ».

Si une entreprise pose problème, tu notes pourquoi et tu passes à la suivante.

## Le récapitulatif final

```
Ajoutés (X) :
- Entreprise : prénom nom, email ou « pas d'email », lien HubSpot

Arrêtés (X) :
- Entreprise : raison
```

Tu n'affiches une rubrique que si elle contient au moins une entreprise. Pour une entreprise ajoutée sans email personnel, tu indiques l'email général s'il y en a un. Si des crédits Apollo ont été utilisés, tu donnes le total.
