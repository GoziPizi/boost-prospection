---
description: Vérifie si un prospect existe déjà dans HubSpot (transaction ou contact) avant de faire des recherches ou de l'ajouter. Utilise ce skill quand il faut savoir si une entreprise ou un contact est déjà dans le CRM.
---

# Vérifier les doublons dans HubSpot

Le prospect à vérifier : $ARGUMENTS

## Ton objectif

Tu cherches à savoir si le prospect est déjà dans HubSpot.

Tu ne modifies jamais rien dans HubSpot : tu fais uniquement des recherches.

## Ce que tu utilises

Tu utilises les informations disponibles parmi :

- le nom de l'entreprise (obligatoire)
- le site web
- l'email du contact
- le nom et prénom du contact

Tu fais uniquement les vérifications possibles avec ces informations.

## Les vérifications

1. **Transaction** : tu cherches une transaction dont le nom correspond au nom de l'entreprise.
2. **Contact par email** : tu cherches un contact avec le même email.
3. **Contact par nom** : tu cherches un contact avec le même prénom et nom, dans la même entreprise.
4. **Domaine** : tu cherches des contacts dont l'email finit par le domaine du site (par exemple @exemple.fr).

## Comment comparer les noms d'entreprise

Deux noms correspondent si, une fois simplifiés, ils sont identiques. Pour les simplifier :

- tu ignores les majuscules, les accents et la ponctuation
- tu retires les formes juridiques (SAS, SARL, SA, EURL, SASU…)

Par exemple, « Dupont & Fils SAS » et « dupont et fils » correspondent.

Si un nom est proche sans être identique, tu ne le comptes pas comme doublon, mais tu le signales.

## Le résultat attendu

Tu rends toujours ce bloc, avec une seule valeur pour le verdict :

```
Verdict : NOUVEAU | TRANSACTION EXISTANTE | CONTACT EXISTANT
Transaction : [nom et lien] ou Aucune
Contact : [nom, email et lien] ou Aucun
Même domaine : [nombre de contacts] ou Aucun
À signaler : [nom proche, etc.] ou Rien
```

Pour choisir le verdict :

- **TRANSACTION EXISTANTE** : une transaction correspond. Le prospect ne doit pas être ajouté.
- **CONTACT EXISTANT** : aucune transaction, mais le contact existe. Il faut réutiliser ce contact au lieu d'en créer un.
- **NOUVEAU** : rien ne correspond.

Des contacts du même domaine ne changent pas le verdict : tu les signales seulement.