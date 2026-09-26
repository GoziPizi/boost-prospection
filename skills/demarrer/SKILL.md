---
description: Configure le plugin boost-prospection en posant quelques questions (pipeline HubSpot, infos à trouver, Apollo), puis écrit le fichier de config. Utilise ce skill au premier lancement, quand la config est absente, ou pour modifier les réglages.
---

# Configurer boost-prospection

## Ton objectif

Tu poses quelques questions à l'utilisateur, tu testes les connexions, puis tu écris le fichier `config-boost-prospection.md` à la racine du dossier de travail (le dossier ouvert pour cette session).

Ce dossier sert de dossier de prospection : la config y reste d'une conversation à l'autre, et les fichiers de suivi y seront rangés.

## Les règles à respecter

- Pour les choix fermés, tu utilises l'outil AskUserQuestion.
- Tu écris la config uniquement à partir du modèle `${CLAUDE_SKILL_DIR}/modele-config.md`. Tu gardes exactement ses titres et ses lignes : tu remplaces seulement les valeurs entre crochets.
- Tu n'écris rien avant la validation de l'étape 6.

## Étape 1 : dossier de travail et config existante

Si aucun dossier de travail n'est ouvert, tu expliques à l'utilisateur qu'il doit ouvrir son dossier de prospection (un dossier sur son ordinateur, par exemple « Prospection »), puis relancer la commande. Tu t'arrêtes là.

Sinon, tu regardes si `config-boost-prospection.md` existe à la racine de ce dossier.

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
- Page de contact (ajoutée en note sur la transaction HubSpot)
- Standard

## Étape 4 : les critères de sélection

Tu demandes en texte : « As-tu des critères pour ne garder que certaines entreprises ? Par exemple : moins de 50 salariés, un secteur d'activité, une zone géographique. Tu peux répondre non. »

S'il répond non, tu mets « aucun ».

Sinon, tu transformes ses réponses en critères vérifiables avec le registre officiel des entreprises ou le site :

- **l'effectif** (par exemple « moins de 50 salariés »)
- **l'activité** (par exemple « expertise comptable »)
- **la localisation** (ville, département ou région du siège)
- **l'âge de l'entreprise** (par exemple « créée il y a plus de 3 ans »)

Si un critère ne peut pas être vérifié de façon fiable (par exemple « entreprises qui recrutent »), tu le lui expliques et tu proposes de le reformuler ou de le retirer.

Tu lui montres la liste finale des critères et tu lui demandes de la valider.

## Étape 5 : Apollo

Tu demandes s'il veut utiliser Apollo pour trouver l'email du dirigeant quand le site n'en donne pas. Tu précises que chaque email trouvé consomme des crédits Apollo.

Tu ne poses cette question que si « Email » fait partie des infos choisies. Sinon, tu mets « non ».

Si c'est oui, tu testes la connexion Apollo avec un appel qui ne consomme pas de crédits (son profil ou son solde de crédits). Si elle ne répond pas, tu lui expliques qu'il doit connecter Apollo.io dans ses connecteurs claude.ai, et tu mets « non ».

## Étape 6 : validation

Tu montres la config complète, remplie à partir du modèle, et tu demandes : « Je l'enregistre ? ».

S'il demande une modification, tu la fais et tu montres à nouveau la config.

## Étape 7 : écrire la config

Tu écris le fichier `config-boost-prospection.md` à la racine du dossier de travail et tu confirmes en une phrase, en rappelant qu'il faudra ouvrir ce même dossier pour chaque prospection.

Si ce skill a été lancé par prospecter parce que la config manquait, tu le dis : prospecter reprend alors là où il en était.
