---
description: Prospecte une ou quelques entreprises données par leur nom - trouve jusqu'à deux dirigeants et leurs coordonnées, puis crée la transaction et les contacts dans HubSpot. Pour une liste en fichier CSV ou Excel, utiliser prospecter-lot.
disable-model-invocation: true
---

# Prospecter

Les entreprises à prospecter : $ARGUMENTS

Si la liste est vide, demande-la à l'utilisateur. Si l'utilisateur donne un fichier CSV ou Excel, dis-lui d'utiliser plutôt `/boost-prospection:prospecter-lot`.

## Avant de commencer

1. Tu cherches le fichier `config-boost-prospection.md` à la racine du dossier de travail (le dossier ouvert pour cette session).
   - Aucun dossier de travail ouvert : tu demandes à l'utilisateur d'ouvrir son dossier de prospection, et tu t'arrêtes là.
   - Le fichier n'existe pas : tu lances le skill demarrer, puis tu reprends ici.
2. Tu lis les règles d'ajout `${CLAUDE_PLUGIN_ROOT}/regles/ajout-hubspot.md`.
3. Si ce n'est pas déjà fait dans cette session, tu demandes une seule fois avec l'outil AskUserQuestion : « Je crée les transactions et les contacts dans HubSpot sans te demander de confirmation pour cette session ? ». Tu ne reposes jamais cette question pendant la session.

## Pour chaque entreprise

Tu traites les entreprises une par une.

1. Avec l'outil Agent, tu lances le sous-agent `boost-prospection:chercheur-prospect` en lui donnant le nom de l'entreprise, les infos à trouver et le réglage Apollo de la config. Tu ne fais aucune recherche toi-même.
2. Fiche ARRÊTÉ : tu notes la raison et tu passes à l'entreprise suivante.
3. Fiche TROUVÉ : tu l'ajoutes dans HubSpot en suivant les règles d'ajout.

Si une entreprise pose problème, tu notes pourquoi et tu passes à la suivante.

## Le récapitulatif final

```
Ajoutés (X) :
- Entreprise : prénom nom (fonction, email ou « pas d'email »), et le deuxième dirigeant s'il y en a un, lien HubSpot

Arrêtés (X) :
- Entreprise : raison
```

Tu n'affiches une rubrique que si elle contient au moins une entreprise. Pour une entreprise ajoutée sans email personnel, tu indiques l'email général s'il y en a un. Si des crédits Apollo ont été utilisés, tu donnes le total.
