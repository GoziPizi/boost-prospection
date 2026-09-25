# Configuration boost-prospection

Ce fichier est lu par les skills du plugin. Pour changer un réglage, modifie la valeur après les deux-points.

## Général

- Validation avant HubSpot : [oui | non]
- Mode silencieux : [oui | non]

## Étapes

### recherche-web
- Activée : oui

### verification-doublon-hubspot
- Activée : [oui si ajout-hubspot est activée, sinon non]

### recherche-site
- Activée : oui

### recherche-linkedin
- Activée : [oui | non]

### enrichissement-apollo
- Activée : [oui | non]
- Récupérer l'email : [oui | non]
- Récupérer le téléphone : [oui | non]

### scoring
- Activée : [oui | non]

Critères (un critère par ligne, avec ses points) :
- [critère vérifiable] : [points, négatifs pour une pénalité]

### ajout-hubspot
- Activée : [oui | non]
- Pipeline : [nom exact du pipeline]
- Étape : [nom exact de l'étape]

Champs supplémentaires (information de la fiche → objet.propriété HubSpot) :
- [information de la fiche] → [contact | transaction].[nom interne de la propriété]

### redaction-brouillon
- Activée : [oui | non]
- Canal : [email | linkedin]
- Tutoiement : [oui | non]
- Longueur maximale : [nombre] mots
- Signature : [signature]
- Enregistrer dans HubSpot : [oui | non]

Mon offre :
[ce que l'utilisateur propose, en 2-3 phrases]

Exemple de message :
[un message dont reprendre le style, ou « aucun »]

## Règles d'arrêt

Un prospect est écarté si une de ces règles n'est pas respectée.

- Dirigeant obligatoire : [oui | non]
- Score minimum : [nombre | aucun]
- Écarter si des contacts du même domaine existent : [oui | non]
