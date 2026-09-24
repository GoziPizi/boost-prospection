---
description: Récupère les coordonnées d'un prospect à partir du site web de son entreprise (dirigeant, email, téléphone, standard, page de contact). Utilise ce skill quand l'utilisateur donne l'URL d'un site d'entreprise et veut trouver qui contacter et comment.
---

# Trouver les coordonnées d'un prospect

Le site à analyser : $ARGUMENTS

Si aucune URL n'est donnée, demande-la à l'utilisateur avant de commencer.

## Ton objectif

Tu cherches à trouver, sur le site de l'entreprise :

1. Le nom du dirigeant
2. Son adresse email, si possible
3. Son téléphone direct, si possible
4. L'URL de la page de contact
5. Le numéro du standard téléphonique

## Les règles à respecter

- Tu ne notes que des informations que tu as vraiment lues sur le site.
- Tu n'inventes jamais une adresse email, même si tu devines le format (par exemple prenom.nom@entreprise.fr).
- Si tu ne trouves pas une information, tu écris « Non trouvé ».
- Pour chaque information trouvée, tu gardes l'URL de la page où tu l'as vue.
- Tu lis les pages avec l'outil WebFetch.

## Étape 1 : la page d'accueil

Tu ouvres la page d'accueil du site.

Tu repères les liens du menu et du bas de page (le footer). Tu cherches en particulier les liens vers :

- une page équipe (« Notre équipe », « Qui sommes-nous », « À propos », « L'agence », « Le cabinet »…)
- les mentions légales
- la page de contact

Tu notes au passage le numéro de téléphone s'il apparaît dans l'en-tête ou le footer : c'est souvent le standard.

## Étape 2 : la page équipe

Tu commences par la page équipe, ou la page la plus proche (« À propos », « Qui sommes-nous »…).

Tu cherches le dirigeant : fondateur, gérant, président, directeur général, CEO, associé…

Tu notes aussi son email ou son téléphone direct s'ils apparaissent sur cette page.

## Étape 3 : les mentions légales

Tu vas ensuite voir les mentions légales.

Tu cherches :

- le nom du directeur de la publication, qui est souvent le dirigeant
- les adresses email qui apparaissent
- le numéro de téléphone

Si la page équipe ne donnait pas de dirigeant, le directeur de la publication est une bonne piste.

## Étape 4 : la page de contact

Tu termines par la page de contact.

Tu notes son URL.

Tu cherches les emails et les numéros de téléphone qui y sont affichés.

Si la page ne contient qu'un formulaire, tu le précises.

## Comment choisir entre plusieurs résultats

- Pour le standard : tu prends le numéro général de l'entreprise (celui du footer ou de la page de contact).
- Pour le téléphone du dirigeant : tu ne notes un numéro que s'il est clairement associé à lui, ou qu'il n'y a aucune ambigüité sur son attribution.
- Pour l'email : tu préfères l'email personnel du dirigeant. Si tu n'en trouves pas, tu donnes l'email général (contact@, info@…) en précisant qu'il est général.
- Si deux pages se contredisent, tu donnes les deux et tu le signales.

## Le résultat attendu

Tu rends un tableau comme celui-ci :

| Information | Valeur | Trouvée sur |
| --- | --- | --- |
| Dirigeant | Nom Prénom (fonction) | URL de la page |
| Email | adresse (personnelle ou générale) | URL de la page |
| Téléphone du dirigeant | numéro | URL de la page |
| Page de contact | URL | — |
| Standard | numéro | URL de la page |

Sous le tableau, tu ajoutes une ou deux phrases maximum si quelque chose mérite d'être signalé (information douteuse, page introuvable, site en maintenance…).
