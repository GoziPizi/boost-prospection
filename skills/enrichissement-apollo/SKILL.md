---
description: Complète les informations du dirigeant d'un prospect avec Apollo (fonction, LinkedIn, email et téléphone selon la config). Utilise ce skill quand on veut enrichir une fiche prospect avec les données Apollo.
---

# Enrichir un prospect avec Apollo

Le prospect : $ARGUMENTS

## Ton objectif

Tu utilises les outils Apollo pour compléter les informations du dirigeant.

Il te faut au minimum le nom de l'entreprise ou son site. Le nom du dirigeant est fortement conseillé.

## La config

Dans `${CLAUDE_PLUGIN_DATA}/config.md`, section enrichissement-apollo, tu lis ce qu'il faut demander à Apollo :

- Récupérer l'email : oui / non
- Récupérer le téléphone : oui / non

Tu ne demandes que les informations marquées « oui ». Si une ligne est absente, tu considères qu'elle vaut « non ».

## Les règles à respecter

- Tu ne demandes jamais à Apollo une information marquée « non » dans la config : l'email et le téléphone consomment des crédits.
- Tu ne demandes pas non plus une information déjà présente dans la fiche.
- Tu ne remplaces jamais une information déjà présente dans la fiche. Tu complètes seulement les champs vides.
- Si Apollo donne une valeur différente d'une valeur déjà présente, tu gardes celle de la fiche et tu le signales.
- Tu n'inventes aucune information.

## Étape 1 : trouver le dirigeant dans Apollo

Si tu connais le nom du dirigeant, tu le cherches avec le nom de l'entreprise ou le domaine du site.

Sinon, tu cherches dans l'entreprise la personne au poste le plus élevé (fondateur, gérant, président, CEO, directeur général).

Si plusieurs personnes correspondent et que tu ne peux pas les départager, tu n'en choisis aucune : verdict INCERTAIN.

## Étape 2 : récupérer les informations

Pour la personne trouvée, tu récupères toujours :

- le prénom et le nom
- la fonction
- l'URL LinkedIn

Et selon la config :

- l'email
- le téléphone

## Le résultat attendu

Tu rends toujours ce bloc :

```
Verdict : ENRICHI | INCERTAIN | INTROUVABLE
Dirigeant : [prénom nom] ou Non trouvé
Fonction : [fonction] ou Non trouvé
LinkedIn dirigeant : [URL] ou Non trouvé
Email : [adresse] ou Non trouvé ou Non demandé
Téléphone : [numéro] ou Non trouvé ou Non demandé
À signaler : [valeurs différentes de la fiche, etc.] ou Rien
```
