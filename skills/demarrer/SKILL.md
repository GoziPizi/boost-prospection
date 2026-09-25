---
description: Configure le plugin boost-prospection en dialoguant avec l'utilisateur, étape par étape, puis écrit le fichier de config. Utilise ce skill au premier lancement, quand la config est absente, ou quand l'utilisateur veut modifier ses réglages de prospection.
---

# Configurer boost-prospection

## Ton objectif

Tu poses des questions à l'utilisateur jusqu'à avoir tout ce qu'il faut pour remplir la config, tu testes les connexions, puis tu écris le fichier `${CLAUDE_PLUGIN_DATA}/config.md`.

## Les règles à respecter

- Tu poses une seule série de questions à la fois, étape par étape.
- Pour les choix fermés (oui / non, listes), tu utilises l'outil AskUserQuestion. Pour les réponses libres (offre, critères, signature), tu demandes en texte.
- Tu écris la config uniquement à partir du modèle `${CLAUDE_SKILL_DIR}/modele-config.md`. Tu gardes exactement ses titres, ses lignes et leur ordre : tu remplaces seulement les valeurs entre crochets.
- Tu n'écris rien avant la validation de l'étape 6.

## Étape 1 : config existante ?

Tu regardes si `${CLAUDE_PLUGIN_DATA}/config.md` existe.

- Elle n'existe pas : tu fais toutes les étapes suivantes.
- Elle existe : tu la résumes en quelques lignes et tu demandes ce que l'utilisateur veut modifier (une étape précise, les règles d'arrêt, ou tout refaire). Tu ne fais ensuite que les étapes concernées, et tu gardes toutes les autres valeurs.

## Étape 2 : les réglages généraux

Tu demandes :

- Validation avant HubSpot : veut-il valider chaque fiche avant l'ajout dans HubSpot ?
- Mode silencieux : veut-il seulement le récapitulatif final, sans suivi prospect par prospect ?

## Étape 3 : les étapes de prospection

Les étapes recherche-web et recherche-site sont toujours activées : tu ne poses pas de question.

Pour chaque étape suivante, tu demandes d'abord : « Tu veux l'activer ? ». Si c'est non, tu passes à la suivante.

### ajout-hubspot

Si activée :

1. Tu testes la connexion HubSpot et tu vérifies que l'utilisateur a le droit de créer des transactions, des contacts et des notes. Si ce n'est pas le cas, tu expliques le problème et tu proposes de désactiver l'étape.
2. Tu listes ses pipelines de transactions et tu lui fais choisir le pipeline, puis l'étape.
3. Tu demandes s'il veut enregistrer d'autres informations de la fiche dans HubSpot (score, LinkedIn entreprise, standard, page de contact…). Pour chacune, tu lui demandes dans quelle propriété, puis tu vérifies qu'elle existe dans HubSpot.
   - Elle existe : tu notes son nom interne.
   - Elle n'existe pas : tu proposes les propriétés au nom proche. S'il n'y en a aucune, tu lui expliques qu'il doit la créer dans HubSpot, et tu ne l'ajoutes pas à la config.

verification-doublon-hubspot suit ajout-hubspot : activée si ajout-hubspot l'est, désactivée sinon. Tu ne poses pas de question.

### recherche-linkedin

Rien de plus à demander.

### enrichissement-apollo

Si activée :

1. Tu testes la connexion Apollo avec un appel qui ne consomme pas de crédits (par exemple une recherche d'entreprise). Si elle ne répond pas, tu expliques le problème et tu proposes de désactiver l'étape.
2. Tu demandes s'il veut récupérer l'email, puis le téléphone, en rappelant que chacun consomme des crédits Apollo.

### scoring

Si activée, tu construis les critères avec l'utilisateur :

1. Tu lui demandes ce qui fait un bon prospect pour lui.
2. Tu transformes ses réponses en critères vérifiables avec la fiche ou la page d'accueil du site. Par exemple, « bonne entreprise » devient « secteur BTP » ou « moins de 50 salariés ».
3. Tu proposes un nombre de points pour chaque critère, et des pénalités s'il y en a. Il valide ou corrige.
4. Tu demandes le score minimum pour garder un prospect.

### redaction-brouillon

Si activée, tu demandes :

- le canal (email ou LinkedIn)
- tutoiement ou vouvoiement
- la longueur maximale
- la signature
- son offre, en 2-3 phrases
- un exemple de message qu'il aime (facultatif)
- s'il veut enregistrer le brouillon en note dans HubSpot, seulement si ajout-hubspot est activée. Sinon, tu mets « non ».

## Étape 4 : les règles d'arrêt

Tu demandes :

- Dirigeant obligatoire : écarter les prospects sans dirigeant trouvé ?
- Écarter si des contacts du même domaine existent : seulement si ajout-hubspot est activée. Sinon, tu mets « non ».

Le score minimum a déjà été demandé avec le scoring. Si le scoring est désactivé, tu mets « aucun ».

## Étape 5 : les connexions

Tu fais le point sur les tests des étapes précédentes. Si une connexion a échoué et que l'étape est restée activée, tu le signales clairement.

## Étape 6 : validation

Tu montres la config complète, remplie à partir du modèle, et tu demandes : « Je l'enregistre ? ».

S'il demande une modification, tu la fais et tu montres à nouveau la config.

## Étape 7 : écrire la config

Tu écris le fichier `${CLAUDE_PLUGIN_DATA}/config.md`.

Tu confirmes en une phrase. Si ce skill a été lancé par prospecter parce que la config manquait, tu le dis : prospecter reprend alors là où il en était.
