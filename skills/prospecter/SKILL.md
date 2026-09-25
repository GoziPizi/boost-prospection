---
description: Prospecte une ou plusieurs entreprises à partir de leur nom - trouve le dirigeant et ses coordonnées sur le site, complète avec LinkedIn et Apollo si besoin, puis crée la transaction et le contact dans HubSpot.
disable-model-invocation: true
---

# Prospecter

Les entreprises à prospecter : $ARGUMENTS

Si la liste est vide, demande-la à l'utilisateur.

## La config

Tu lis `${CLAUDE_PLUGIN_DATA}/config.md`. Si elle n'existe pas, tu lances le skill demarrer, puis tu reprends ici.

Tu y lis :

- le pipeline et l'étape HubSpot
- les infos à trouver
- si Apollo est utilisé

## Avant de commencer

Une seule fois, avant la première entreprise, tu demandes avec l'outil AskUserQuestion :

« Je crée les transactions et les contacts dans HubSpot sans te demander de confirmation pour cette session ? »

- **Oui** : l'utilisateur lève les confirmations HubSpot pour toute la session. Tu ne montres plus aucun tableau de confirmation et tu ne poses plus aucune question avant les ajouts. Pour les outils HubSpot qui demandent un statut de confirmation, tu indiques que les confirmations sont levées pour la session.
- **Non** : avant chaque ajout, tu montres ce que tu vas créer et tu attends son accord. S'il refuse : arrêt, raison « refusé par l'utilisateur ».

Tu ne reposes jamais cette question pendant la session.

## Les règles à respecter

- Tu n'inventes jamais une information. Une info non trouvée reste vide.
- Tu n'ouvres jamais deux fois la même page.
- Dès que tu as toutes les infos à trouver, tu arrêtes de chercher et tu passes à l'étape suivante.
- Quand tu ouvres une page avec WebFetch, tu lui demandes uniquement les infos qui te manquent encore, et les liens vers les pages de l'étape 2.
- Si une entreprise pose problème, tu notes pourquoi et tu passes à la suivante. Tu ne t'arrêtes jamais pour tout le lot.

## Pour chaque entreprise

Tu traites les entreprises une par une.

### Étape 1 : vérifier les doublons

Tu cherches dans HubSpot une transaction dont le nom correspond au nom de l'entreprise. Tu ignores les majuscules, les accents, la ponctuation et les formes juridiques (SAS, SARL…).

Si elle existe : arrêt, raison « déjà dans HubSpot ».

### Étape 2 : chercher sur le site

1. Tu cherches le site officiel avec WebSearch. Tu écartes les annuaires (societe.com, pappers, pagesjaunes…), les réseaux sociaux et la presse. Si tu ne peux pas choisir entre plusieurs sites : arrêt, raison « site introuvable ou incertain ».
2. Tu visites les pages dans cet ordre, en t'arrêtant dès que tu as toutes les infos :
   1. **Accueil** : les liens du menu et du bas de page, et le numéro de téléphone affiché (souvent le standard).
   2. **Équipe** (« Notre équipe », « À propos », « Qui sommes-nous »…) : le dirigeant (fondateur, gérant, président, CEO, directeur général, associé), et son email, son téléphone ou son LinkedIn s'ils sont affichés.
   3. **Mentions légales** : le directeur de la publication, souvent le dirigeant, et les emails affichés.
   4. **Contact** : l'URL de la page, les emails et les téléphones affichés.

Si une page n'existe pas, tu passes à la suivante.

Pour l'email, tu préfères l'email personnel du dirigeant. Un email général (contact@, info@…) compte comme « non trouvé » pour le dirigeant, mais tu le gardes pour le récapitulatif.

### Étape 3 : LinkedIn

Seulement si tu n'as pas de dirigeant, ou si « LinkedIn du dirigeant » est une info à trouver et qu'il manque.

- **Pas de dirigeant** : tu cherches `site:linkedin.com/in "nom de l'entreprise" gérant OR fondateur OR président OR CEO`. Tu ne gardes un nom que si le titre du résultat montre clairement l'entreprise et un poste de direction. Toujours rien : arrêt, raison « dirigeant introuvable ».
- **Dirigeant connu, LinkedIn manquant** : tu cherches `site:linkedin.com/in prénom nom entreprise`. Tu ne gardes l'URL que si le nom et l'entreprise correspondent.

Tu n'ouvres jamais une page LinkedIn : tu te fies au titre et à la description du résultat.

### Étape 4 : Apollo

Seulement si Apollo est activé, que tu as le dirigeant et qu'il te manque son email personnel.

Tu cherches le dirigeant dans Apollo avec son nom et le domaine du site, et tu demandes son email. Si Apollo trouve une autre personne, tu ne gardes rien.

### Étape 5 : ajouter dans HubSpot

1. Tu cherches un contact avec le même email. S'il existe, tu le réutilises au lieu d'en créer un.
2. Tu crées la transaction : nom = nom de l'entreprise, pipeline et étape de la config. Tu retrouves leurs identifiants à partir de leurs noms.
3. Tu crées le contact avec les infos trouvées :

| Info | Propriété HubSpot |
| --- | --- |
| Prénom | firstname |
| Nom | lastname |
| Fonction | jobtitle |
| Email personnel | email |
| Téléphone direct, sinon standard | phone |
| LinkedIn du dirigeant | hs_linkedin_url |
| Nom de l'entreprise | company |
| Site | website |

4. Tu associes le contact à la transaction.
5. Tu écris : « Nouvelle transaction créée : [entreprise], avec le contact : [prénom nom] ».

Tu ne mets jamais un email général sur le contact.

## Le récapitulatif final

Quand toutes les entreprises sont traitées :

```
Ajoutés (X) :
- Entreprise : prénom nom, email ou « pas d'email », lien HubSpot

Arrêtés (X) :
- Entreprise : raison
```

Tu n'affiches une rubrique que si elle contient au moins une entreprise. Si tu as trouvé un email général pour une entreprise ajoutée sans email personnel, tu l'indiques à côté.
