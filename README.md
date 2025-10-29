# Guide d'Utilisation API-sms http

## API HTTP SMS
L’API HTTP SMS d’AlgerieSMS vous permet d’envoyer des SMS, de vérifier leur livraison, de consulter votre solde et de gérer vos noms d’expéditeur à l’aide de liens web (URLs). Vous aurez besoin d’une Clé d’Autorisation et d’un Nom d’Expéditeur pour l’utiliser. Ces informations sont fournies lorsque vous créez une application sur le site d’AlgerieSMS.

## Vos Détails API
Lorsque vous créez une application sur https://app.algeriesms.com, vous recevez :

- **ID d’Application**: Un numéro unique pour votre application (ex.: 26).
- **Clé d’Autorisation**: Un code secret pour accéder à l’API (gardez-le confidentiel !).
- **Nom d’Expéditeur**: Le nom qui apparaît comme expéditeur de vos SMS (ex.: "MonEntreprise").
- **URLs API**: Les liens pour envoyer des SMS, vérifier la livraison, etc.

### Exemple de Détails
- **Nom de l’Application**: baylacen.elabed@gmail.com
- **Clé d’Autorisation**: (Votre clé secrète, affichée comme ••••• dans le tableau de bord)
- **Nom d’Expéditeur**: YYYYYYY (remplacez par votre nom d’expéditeur approuvé)
- **Date d’Expiration**: 2026-01-21 10:26
usage.md
markdown# Comment Utiliser l’API

## 1. Envoyer un SMS
L’API fonctionne en envoyant des requêtes à des URLs spécifiques. Vous remplacez les placeholders par vos valeurs réelles. Vous pouvez tester ces URLs dans un navigateur, un outil comme Postman, ou un script de programmation.

### URL
https://api.l2t.io/dz/s/api/v1/sms?fct=sms&key=%KEY%&mobile=%MOBILE%&sms=%SMS%&sender=%SENDER%&date=%DATE%&heure=%HEURE%&content-type=%CONTENT-TYPE%
text- `%KEY%`: Votre Clé d’Autorisation.
- `%MOBILE%`: Le numéro de téléphone du destinataire au format international (ex.: 21612345678 pour la Tunisie, sans "+" ni "00").
- `%SMS%`: Le message que vous souhaitez envoyer (ex.: "Bonjour, ceci est un test !").
- `%SENDER%`: Votre nom d’expéditeur approuvé (ex.: "MonEntreprise").
- `%DATE%`: (Facultatif) La date d’envoi du SMS (format: jj/mm/aaaa, ex.: 25/10/2025).
- `%HEURE%`: (Facultatif) L’heure d’envoi du SMS (format: hh:mm, ex.: 14:30).
- `%CONTENT-TYPE%`: (Facultatif) Utilisez "JSON" ou "XML" (par défaut: JSON).

## 1.1 Exemple D’envoi d’un SMS
https://api.l2t.io/dz/s/api/v1/sms?fct=sms&key=votre_clé_ici&mobile=21612345678&sms=Bonjour+Monde&sender=MonEntreprise&date=25/10/2025&heure=14:30&content-type=JSON
text### Réponse
```json
{
  "success": true,
  "message": "le message est enregistré dans la file d’attente",
  "code": "message_queued",
  "status": "200",
  "data": {
    "message_id": 12345,
    "msisdn": "21612345678",
    "ref": "abc123"
  }
}
Cela signifie que votre SMS est en attente d’envoi. Conservez le message_id pour vérifier la livraison plus tard.
Conseils

Utilisez "+" à la place des espaces dans le texte du SMS (ex.: "Bonjour+Monde").
Pour les messages longs, contactez le support d’AlgerieSMS pour activer cette fonctionnalité.

2. Vérifier l’État de Livraison (DLR)
Pour vérifier si votre SMS a été livré, utilisez cette URL:
URL
texthttps://api.l2t.io/dz/s/api/v1/sms?fct=dlr&key=%KEY%&msg_id=%MSG_ID%&content-type=%CONTENT-TYPE%

%KEY%: Votre Clé d’Autorisation.
%MSG_ID%: Le message_id obtenu lors de l’envoi du SMS (ex.: 12345;67890 pour plusieurs IDs, séparés par ";").
%CONTENT-TYPE%: (Facultatif) Utilisez "JSON" ou "XML" (par défaut: JSON).

2.2 Exemple De Vérification l’État de Livraison (DLR)
texthttps://api.l2t.io/dz/s/api/v1/sms?fct=dlr&key=votre_clé_ici&msg_id=12345&content-type=JSON
Réponse
json{
  "success": true,
  "message": "réponse dlr",
  "code": "dlr_success",
  "status": 200,
  "data": [
    {
      "message_id": 12345,
      "dlr": "DELIVERED",
      "dlr_date": "2025-10-21 10:30",
      "ref": "abc123"
    }
  ]
}
Signification des États de Livraison

DELIVERED: Le SMS a été livré au destinataire.
UNDELIVERED: Le SMS n’a pas pu être livré (ex.: numéro incorrect ou téléphone éteint).
EXPIRED: Le SMS n’a pas été livré dans les délais (ex.: destinataire hors couverture).
REJECTED: Le destinataire a bloqué votre SMS (ex.: il a répondu "STOP").
UNKNOWN: Le SMS est en cours de traitement, vérifiez à nouveau plus tard.

3. Consulter Votre Solde
Pour voir combien de crédits SMS il vous reste dans votre compte AlgerieSMS, utilisez cette URL:
URL
texthttps://api.l2t.io/dz/s/api/v1/sms?fct=balance&key=%KEY%&content-type=%CONTENT-TYPE%

%KEY%: Votre Clé d’Autorisation (le code secret affiché comme ••••• dans votre tableau de bord).
%CONTENT-TYPE%: (Facultatif) Utilisez "JSON" ou "XML" pour choisir le format de la réponse (par défaut: JSON).

3.3 Exemple De Consultation de Solde
texthttps://api.l2t.io/dz/s/api/v1/sms?fct=balance&key=votre_clé_ici&content-type=JSON
Réponse
json{
  "success": true,
  "message": "Solde du compte",
  "code": "balance_success",
  "status": 200,
  "data": {
    "id_client": 1234,
    "balance": 500
  }
}
Comprendre la Réponse

success: true: La requête a fonctionné.
message: "Solde du compte": Confirme que vous obtenez des informations sur le solde.
code: "balance_success": Indique que la requête a réussi.
status: 200: Un code standard pour une requête réussie.
data:

id_client: Votre identifiant client unique (ex.: 1234).
balance: Le nombre de crédits SMS restants (ex.: 500). Cela indique combien de SMS vous pouvez envoyer ou peut représenter une valeur monétaire, selon le système d’AlgerieSMS.



3.3 Exemple De Consultation de Solde (Guide Étape par Étape)

Trouver Votre Clé d’Autorisation:

Connectez-vous à https://app.algeriesms.com.
Allez dans la section API et trouvez votre application (nommée baylacen.elabed@gmail.com).
Copiez la Clé d’Autorisation (une longue chaîne de caractères).


Construire l’URL:

Commencez par: https://api.l2t.io/dz/s/api/v1/sms?fct=balance.
Ajoutez votre clé: Remplacez %KEY% par votre Clé d’Autorisation.
(Facultatif) Ajoutez le type de contenu: Ajoutez &content-type=JSON (ou XML).
Exemple: https://api.l2t.io/dz/s/api/v1/sms?fct=balance&key=abc123xyz&content-type=JSON.


Envoyer la Requête:

Collez l’URL dans un navigateur ou un outil comme Postman.
Si vous codez, utilisez un script (ex.: Python) pour envoyer la requête.


Lire la Réponse:

Vérifiez la valeur balance (ex.: 500).
Si votre solde est faible, contactez AlgerieSMS pour ajouter des crédits.



Conseils

Gardez votre Clé d’Autorisation secrète et ne la partagez pas.
Testez l’URL dans Postman si vous débutez avec les API, c’est plus simple que de coder.
Vérifiez votre solde régulièrement pour vous assurer d’avoir assez de crédits.
Si le solde est incorrect ou si vous ne pouvez pas y accéder, contactez le support d’AlgerieSMS.

4. Récupérer la Liste des Expéditeurs
Pour voir vos noms d’expéditeur approuvés, utilisez cette URL:
URL
texthttps://api.l2t.io/dz/s/api/v1/sms?fct=sender&key=%KEY%&content-type=%CONTENT-TYPE%

%KEY%: Votre Clé d’Autorisation.
%CONTENT-TYPE%: (Facultatif) Utilisez "JSON" ou "XML" (par défaut: JSON).

4.4 Exemple De Récupération de la Liste des Expéditeurs
texthttps://api.l2t.io/dz/s/api/v1/sms?fct=sender&key=votre_clé_ici&content-type=JSON
Réponse
json{
  "success": true,
  "message": "Liste des expéditeurs autorisés",
  "code": "sender_success",
  "status": 200,
  "data": [
    {
      "sender": "MonEntreprise",
      "total_mt": 100
    },
    {
      "sender": "TestExpéditeur",
      "total_mt": 50
    }
  ]
}
text### `tips.md`
```markdown
# Conseils pour Réussir

## Protégez Votre Clé
- Ne partagez pas votre Clé d’Autorisation publiquement.

## Testez d’Abord
- Utilisez un outil comme Postman pour tester les URLs avant de coder.

## Contactez le Support
- En cas de problème ou pour envoyer des SMS longs, contactez le support d’AlgerieSMS.

## Utilisez des Numéros Internationaux
- Utilisez toujours les codes de pays sans "+" ni "00" (ex.: 21612345678).