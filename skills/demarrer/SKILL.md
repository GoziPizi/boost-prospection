---
description: Configure le plugin boost-prospection en posant quelques questions (pipeline HubSpot, infos à trouver, Apollo), puis écrit le fichier de config. Utilise ce skill au premier lancement, quand la config est absente, ou pour modifier les réglages.
---

# Configurer boost-prospection

## Ton objectif

Tu poses quelques questions à l'utilisateur, tu testes les connexions, puis tu écris le fichier `${CLAUDE_PLUGIN_DATA}/config.md`.

## Les règles à respecter

- Pour les choix fermés, tu utilises l'outil AskUserQuestion.
- Tu écris la config uniquement à partir du modèle `${CLAUDE_SKILL_DIR}/modele-config.md`. Tu gardes exactement ses titres et ses lignes : tu remplaces seulement les valeurs entre crochets.
- Tu n'écris rien avant la validation de l'étape 5.

## Étape 1 : config existante ?

Tu regardes si `${CLAUDE_PLUGIN_DATA}/config.md` existe.

- Elle n'existe pas : tu fais toutes les étapes.
- Elle existe : tu la résumes en trois lignes et tu demandes ce que l'utilisateur veut modifier. Tu ne fais que les étapes concernées et tu gardes le reste.

## Étape 2 : HubSpot

1. Tu testes la connexion HubSpot et tu vérifies que l'utilisateur peut créer des transactions et des contacts. Si ce n'est pas le cas, tu t'arrêtes et tu lui expliques qu'il doit connecter HubSpot dans ses connecteurs claude.ai.
2. Tu listes ses pipelines de transactions et tu lui fais choisir le pipeline.
3. Tu listes les étapes de ce pipeline et tu lui fais choisir l'étape.

## Étape 3 : les infos à trouver

Le dirigeant est toujours cherché : tu ne poses pas la question.

Tu lui fais choisir les autres infos (plusieurs choix possibles) :

- Email
- Téléphone direct
- LinkedIn du dirigeant
- Page de contact
- Standard

## Étape 4 : Apollo

Tu demandes s'il veut utiliser Apollo pour trouver l'email du dirigeant quand le site n'en donne pas. Tu précises que chaque email trouvé consomme des crédits Apollo.

Tu ne poses cette question que si « Email » fait partie des infos choisies. Sinon, tu mets « non ».

Si c'est oui, tu testes la connexion Apollo avec un appel qui ne consomme pas de crédits (son profil ou son solde de crédits). Si elle ne répond pas, tu lui expliques qu'il doit connecter Apollo.io dans ses connecteurs claude.ai, et tu mets « non ».

## Étape 5 : validation

Tu montres la config complète, remplie à partir du modèle, et tu demandes : « Je l'enregistre ? ».

S'il demande une modification, tu la fais et tu montres à nouveau la config.

## Étape 6 : écrire la config

Tu écris le fichier `${CLAUDE_PLUGIN_DATA}/config.md` et tu confirmes en une phrase.

Si ce skill a été lancé par prospecter parce que la config manquait, tu le dis : prospecter reprend alors là où il en était.
