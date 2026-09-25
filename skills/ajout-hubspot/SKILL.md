---
description: Ajoute un prospect dans HubSpot en créant une transaction au nom de l'entreprise et en y associant le contact principal (nom, prénom, email, téléphone, LinkedIn). Utilise ce skill quand l'utilisateur veut enregistrer un prospect ou une entreprise dans HubSpot, par exemple juste après avoir trouvé ses coordonnées.
---

# Ajouter un prospect dans HubSpot

Le prospect à ajouter : $ARGUMENTS

## Ton objectif

Pour chaque entreprise, tu dois :

1. Créer une transaction au nom de l'entreprise.
2. Créer le contact principal avec toutes ses informations.
3. Associer le contact à la transaction.
4. Prévenir l'utilisateur que c'est fait.

## Les informations dont tu as besoin

Tu récupères les informations du prospect :

- dans la conversation, si le skill contacts-prospect vient d'être lancé
- ou dans ce que l'utilisateur t'a donné

Si tu n'as que l'URL du site, tu lances d'abord le skill contacts-prospect pour trouver les coordonnées.

Il te faut au minimum le nom de l'entreprise et le nom du contact. S'il manque l'un des deux, tu demandes à l'utilisateur.

## Les règles à respecter

- Tu utilises les outils HubSpot.
- Tu n'inventes aucune information : un champ inconnu reste vide.
- Tu ne crées jamais de doublon (voir l'étape 1).
- Si une information vient d'une page web, tu ne la modifies pas.

## Étape 1 : vérifier les doublons

Avant de créer quoi que ce soit, tu cherches dans HubSpot :

- une transaction qui porte déjà le nom de l'entreprise
- un contact qui a déjà le même email, ou le même nom et prénom dans la même entreprise

Si la transaction existe déjà, tu t'arrêtes et tu préviens l'utilisateur, avec le lien vers la transaction existante.

Si le contact existe déjà, tu ne le recrées pas : tu utilises le contact existant à l'étape 4.

## Étape 2 : créer la transaction

Tu crées une transaction avec :

- Nom de la transaction : le nom de l'entreprise
- Pipeline : Prospection LinkedIn
- Étape : A contacter mail

Tu retrouves les identifiants du pipeline et de l'étape avec les outils HubSpot, à partir de ces noms.

## Étape 3 : créer le contact principal

Tu crées le contact avec ces champs HubSpot :

| Information | Champ HubSpot |
| --- | --- |
| Prénom | firstname |
| Nom | lastname |
| Email | email |
| Téléphone | phone |
| LinkedIn | hs_linkedin_url |
| Fonction (gérant, CEO…) | jobtitle |
| Nom de l'entreprise | company |
| Site web | website |

Pour séparer le prénom et le nom, tu te fies à ce qui est écrit sur le site. En cas de doute, tu mets tout dans le nom et tu le signales.

Pour l'email : tu ne mets que l'email personnel du contact. Un email général (contact@, info@…) ne va pas sur le contact : tu le mentionnes seulement dans le message final.

Pour le téléphone : tu mets le téléphone direct du contact. S'il n'y en a pas, tu mets le numéro du standard.

## Étape 4 : associer le contact à la transaction

Tu associes le contact à la transaction que tu viens de créer.

## Étape 5 : prévenir l'utilisateur

Une fois tout ajouté, tu envoies ce message à l'utilisateur :

« Nouvelle transaction créée : [nom de l'entreprise], avec le contact : [prénom et nom du contact] »

Si quelque chose mérite d'être signalé (champ vide, contact déjà existant, email général non ajouté…), tu l'ajoutes en une phrase sous le message.

## Si tu as plusieurs sites

Tu fais les étapes 1 à 5 pour chaque site, l'un après l'autre.

À la fin, tu envoies un message par transaction créée, puis la liste des sites qui n'ont pas pu être ajoutés et pourquoi.
