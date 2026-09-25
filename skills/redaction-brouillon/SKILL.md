---
description: Rédige un brouillon de premier message de prospection (email ou LinkedIn) pour un prospect, selon l'offre et le style définis dans la config. Utilise ce skill quand on veut préparer un message d'approche pour un prospect.
---

# Rédiger un brouillon de message

Le prospect : $ARGUMENTS

## Ton objectif

Tu rédiges un premier message de prospection pour le dirigeant, prêt à être relu par l'utilisateur.

Tu n'envoies jamais le message : tu écris seulement un brouillon.

## La config

Dans `${CLAUDE_PLUGIN_DATA}/config.md`, section redaction-brouillon, tu lis :

- Canal : email ou linkedin
- Tutoiement : oui / non
- Longueur maximale : nombre de mots
- Signature
- Mon offre : ce que l'utilisateur propose
- Exemple de message : un message dont tu reprends le style, s'il y en a un
- Enregistrer dans HubSpot : oui / non

Si « Mon offre » est vide, tu t'arrêtes et tu rends le verdict SANS OFFRE.

## Les règles à respecter

- Tu n'utilises que des faits présents dans la fiche prospect ou sur le site. Tu n'inventes jamais un compliment, un chiffre ou une actualité.
- Tu t'adresses au dirigeant par son nom. Si la fiche n'a pas de dirigeant, tu rends le verdict SANS DESTINATAIRE.
- Tu respectes la longueur maximale.
- Tu ne mentionnes jamais comment tu as trouvé ses coordonnées, ni son score.

## Étape 1 : trouver l'accroche

Tu ouvres la page d'accueil du site avec WebFetch, si ce n'est pas déjà fait.

Tu choisis un seul détail concret et vérifiable sur l'entreprise (activité, spécialité, ville, réalisation mise en avant). C'est lui qui rend le message personnel.

## Étape 2 : rédiger le message

Le message suit cet ordre :

1. Une phrase d'accroche avec le détail choisi.
2. Le lien entre ce détail et l'offre.
3. Une question simple pour inviter à répondre (par exemple, un échange de 15 minutes).
4. La signature.

Selon le canal :

- **email** : tu ajoutes un objet court, sans majuscules abusives ni point d'exclamation.
- **linkedin** : pas d'objet, et 300 caractères maximum (limite d'une invitation LinkedIn).

S'il y a un exemple de message dans la config, tu reprends son ton et sa structure, sans copier ses phrases.

## Étape 3 : enregistrer dans HubSpot (si activé)

Si « Enregistrer dans HubSpot » vaut oui et que le contact a un lien HubSpot dans la fiche, tu ajoutes le brouillon en note sur ce contact, avec le titre « Brouillon de prospection ».

## Le résultat attendu

Tu rends toujours ce bloc :

```
Verdict : RÉDIGÉ | SANS OFFRE | SANS DESTINATAIRE
Canal : email | linkedin
Objet : [objet] ou —
Message :
[message complet]
Accroche utilisée : [le détail choisi et la page où tu l'as vu]
Enregistré dans HubSpot : oui | non
```
