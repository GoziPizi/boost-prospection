---
description: Attribue un score à un prospect selon les critères définis dans la config. Utilise ce skill quand on veut évaluer ou classer un prospect.
---

# Scorer un prospect

Le prospect : $ARGUMENTS

## Ton objectif

Tu donnes un score au prospect en appliquant les critères de la config.

Tu ne décides pas si le prospect est gardé ou écarté : tu donnes seulement le score.

## La config

Dans `${CLAUDE_PLUGIN_DATA}/config.md`, section scoring, tu lis la liste des critères. Chaque critère a un nombre de points, qui peut être négatif.

S'il n'y a aucun critère, tu t'arrêtes et tu rends le verdict SANS CRITÈRES.

## Étape 1 : évaluer chaque critère

Pour chaque critère, tu décides s'il est rempli, en te basant sur la fiche prospect.

Si la fiche ne suffit pas (secteur, taille de l'entreprise…), tu peux ouvrir la page d'accueil du site avec WebFetch. Tu ne fais pas d'autre recherche.

Chaque critère reçoit un de ces résultats :

- **Oui** : le critère est rempli, tu comptes ses points.
- **Non** : le critère n'est pas rempli, 0 point.
- **Inconnu** : tu n'as pas l'information, 0 point.

En cas de doute, tu mets « Inconnu », jamais « Oui ».

## Étape 2 : calculer le score

Le score est la somme des points des critères à « Oui ».

Le maximum possible est la somme des points positifs.

## Le résultat attendu

Tu rends toujours ce bloc :

```
Verdict : SCORÉ | SANS CRITÈRES
Score : [score] / [maximum]
Détail :
- [critère] : Oui (+20) | Non (0) | Inconnu (0)
- …
Inconnus : [nombre de critères inconnus]
```
