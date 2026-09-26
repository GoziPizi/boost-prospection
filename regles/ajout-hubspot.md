# Règles d'ajout dans HubSpot

Ces règles sont suivies par les skills prospecter et prospecter-lot, pour chaque fiche au statut TROUVÉ renvoyée par le sous-agent `boost-prospection:chercheur-prospect`.

## Les confirmations

Tu appliques le choix fait par l'utilisateur au début de la session :

- **Sans confirmation** : l'utilisateur a levé les confirmations HubSpot pour toute la session. Tu ne montres aucun tableau de confirmation et tu ne poses aucune question. Pour les outils HubSpot qui demandent un statut de confirmation, tu indiques que les confirmations sont levées pour la session.
- **Avec confirmation** : avant de créer quoi que ce soit, tu montres la transaction et le contact que tu vas créer, et tu attends son accord. S'il refuse : arrêt, raison « refusé par l'utilisateur ».

## Les étapes

1. Tu vérifies une dernière fois qu'aucune transaction ne porte déjà le nom de l'entreprise. Si c'est le cas : arrêt, raison « déjà dans HubSpot ».
2. Si la fiche a un email, tu cherches un contact avec cet email. S'il existe, tu le réutilises au lieu d'en créer un.
3. Tu crées la transaction : nom = nom de l'entreprise, pipeline et étape de la config `${CLAUDE_PLUGIN_DATA}/config.md`. Tu retrouves leurs identifiants à partir de leurs noms.
4. Tu crées le contact avec les infos de la fiche :

| Info de la fiche | Propriété HubSpot |
| --- | --- |
| Prénom | firstname |
| Nom | lastname |
| Fonction | jobtitle |
| Email | email |
| Téléphone direct, sinon Standard | phone |
| LinkedIn | hs_linkedin_url |
| Entreprise | company |
| Site | website |

5. Tu associes le contact à la transaction.

Tu ne mets jamais l'email général sur le contact. Un champ vide dans la fiche reste vide dans HubSpot.

## Le résultat

Tu gardes le lien de la transaction créée pour le récapitulatif.
