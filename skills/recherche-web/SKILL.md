---
description: Trouve le site officiel d'une entreprise à partir de son nom, et sa page LinkedIn si l'étape LinkedIn est activée. Utilise ce skill quand on a le nom d'une entreprise mais pas l'URL de son site.
---

# Rechercher une entreprise sur internet

L'entreprise à rechercher : $ARGUMENTS

## Ton objectif

Tu cherches à trouver :

1. l'URL du site officiel de l'entreprise
2. l'URL de sa page LinkedIn entreprise, seulement si l'étape recherche-linkedin est activée dans la config (`${CLAUDE_PLUGIN_DATA}/config.md`)

Si l'URL du site est déjà connue, tu ne la cherches pas : tu passes directement à LinkedIn.

## Les règles à respecter

- Tu utilises WebSearch pour chercher et WebFetch pour vérifier.
- Tu n'inventes jamais une URL.
- En cas de doute, tu ne choisis pas au hasard : tu rends le verdict INCERTAIN.

## Étape 1 : trouver le site

Tu cherches le nom de l'entreprise. Si tu as d'autres informations (ville, secteur), tu les ajoutes à la recherche.

Tu écartes les sites qui ne sont pas celui de l'entreprise : annuaires (societe.com, pappers, pagesjaunes…), réseaux sociaux, articles de presse, places de marché.

## Étape 2 : vérifier le site

Tu ouvres le site trouvé et tu vérifies que c'est bien l'entreprise recherchée : son nom apparaît sur la page, et l'activité correspond si tu la connais.

Si plusieurs entreprises portent le même nom et que tu ne peux pas les départager, le verdict est INCERTAIN.

## Étape 3 : trouver la page LinkedIn (si activée)

Tu cherches la page LinkedIn entreprise, par exemple avec la recherche `site:linkedin.com/company nom de l'entreprise`.

Tu ne cherches pas à ouvrir la page LinkedIn : elle demande une connexion. Tu te fies au titre et à la description du résultat de recherche.

Tu ne gardes la page que si le nom correspond clairement.

## Le résultat attendu

Tu rends toujours ce bloc :

```
Verdict : TROUVÉ | INCERTAIN | INTROUVABLE
Site : [URL] ou Non trouvé
LinkedIn entreprise : [URL] ou Non trouvé ou Non recherché
Candidats : [liste des URLs possibles, seulement si INCERTAIN]
```

Pour choisir le verdict :

- **TROUVÉ** : le site est vérifié.
- **INCERTAIN** : plusieurs sites possibles, ou aucun ne peut être vérifié avec certitude.
- **INTROUVABLE** : aucun site officiel trouvé.

Le verdict concerne uniquement le site : une page LinkedIn non trouvée ne le change pas.
