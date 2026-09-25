---
description: Trouve l'URL du profil LinkedIn du dirigeant d'une entreprise. Utilise ce skill quand on connaît le nom du dirigeant et de son entreprise, et qu'on cherche son profil LinkedIn.
---

# Rechercher le LinkedIn du dirigeant

Le prospect : $ARGUMENTS

## Ton objectif

Tu cherches l'URL du profil LinkedIn du dirigeant.

Il te faut le prénom et le nom du dirigeant, et le nom de l'entreprise. S'il manque l'un des deux, tu ne cherches pas : tu rends « Non recherché ».

## Les règles à respecter

- Tu utilises WebSearch.
- Tu n'ouvres jamais la page LinkedIn : elle demande une connexion. Tu te fies au titre et à la description du résultat de recherche.
- Tu n'inventes jamais une URL.
- En cas de doute, tu ne choisis pas au hasard : tu rends le verdict INCERTAIN.

## Étape 1 : chercher le profil

Tu lances cette recherche :

`site:linkedin.com/in prénom nom entreprise`

Si rien ne correspond, tu relances sans le nom de l'entreprise, mais avec sa ville ou son secteur si tu les connais.

## Étape 2 : vérifier le profil

Tu gardes un profil seulement si :

- le prénom et le nom correspondent
- et l'entreprise ou la fonction apparaît dans le titre ou la description

Un homonyme dans une autre entreprise ne compte pas.

## Le résultat attendu

Tu rends toujours ce bloc :

```
Verdict : TROUVÉ | INCERTAIN | INTROUVABLE | NON RECHERCHÉ
LinkedIn dirigeant : [URL] ou Non trouvé
Candidats : [liste des URLs possibles, seulement si INCERTAIN]
```

Pour choisir le verdict :

- **TROUVÉ** : un seul profil correspond, nom et entreprise vérifiés.
- **INCERTAIN** : plusieurs profils possibles, ou le nom correspond sans que l'entreprise soit confirmée.
- **INTROUVABLE** : aucun profil ne correspond.
- **NON RECHERCHÉ** : il manquait le nom du dirigeant ou de l'entreprise.
