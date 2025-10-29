# Guide d'Utilisation API-sms HTTP

## API HTTP SMS
L’API HTTP SMS d’AlgerieSMS vous permet d’envoyer des SMS, de vérifier leur livraison, de consulter votre solde et de gérer vos noms d’expéditeur à l’aide de liens web (URLs). Vous aurez besoin d’une **Clé d’Autorisation** et d’un **Nom d’Expéditeur** pour l’utiliser. Ces informations sont fournies lorsque vous créez une application sur le site d’AlgerieSMS.

---

## Vos Détails API
Lorsque vous créez une application sur [https://app.algeriesms.com](https://app.algeriesms.com), vous recevez :

-  🆔   **ID d’Application**: Un numéro unique pour votre application (ex.: 26).  
- 🔑  **Clé d’Autorisation**: Un code secret pour accéder à l’API (gardez-le confidentiel !).  
- 📝  **Nom d’Expéditeur**: Le nom qui apparaît comme expéditeur de vos SMS (ex.: "MonEntreprise").  
- 🔗  **URLs API**: Les liens pour envoyer des SMS, vérifier la livraison, etc.

### Exemple de Détails
- **Nom de l’Application**: baylacen.elabed@gmail.com  
- **Clé d’Autorisation**: (Votre clé secrète, affichée comme ••••• dans le tableau de bord)  
- **Nom d’Expéditeur**: YYYYYYY (remplacez par votre nom d’expéditeur approuvé)  
- **Date d’Expiration**: 2026-01-21 10:26  

---

# Comment Utiliser l’API

## 1. Envoyer un SMS
L’API fonctionne en envoyant des requêtes à des URLs spécifiques. Vous remplacez les placeholders par vos valeurs réelles. Vous pouvez tester ces URLs dans un navigateur, un outil comme **Postman**, ou un script de programmation.

  ###  🔗 URL
https://api.l2t.io/dz/s/api/v1/sms?fct=sms&key=%KEY%&mobile=%MOBILE%&sms=%SMS%&sender=%SENDER%&date=%DATE%&heure=%HEURE%&content-type=%CONTENT-TYPE%



### Paramètres
- `%KEY%`: Votre Clé d’Autorisation.  
- `%MOBILE%`: Numéro du destinataire (ex.: 21612345678).  
- `%SMS%`: Le message à envoyer (ex.: "Bonjour, ceci est un test !").  
- `%SENDER%`: Nom d’expéditeur approuvé.  
- `%DATE%`: (Facultatif) Date d’envoi (jj/mm/aaaa).  
- `%HEURE%`: (Facultatif) Heure d’envoi (hh:mm).  
- `%CONTENT-TYPE%`: (Facultatif) "JSON" ou "XML" (par défaut: JSON).

### Exemple d’envoi
https://api.l2t.io/dz/s/api/v1/sms?fct=sms&key=votre_clé_ici&mobile=21612345678&sms=Bonjour+Monde&sender=MonEntreprise&date=25/10/2025&heure=14:30&content-type=JSON



### Réponse
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
```
Cela signifie que votre SMS est en attente d’envoi. Conservez le message_id pour vérifier la livraison plus tard.

2. Vérifier l’État de Livraison (DLR)
### 🔗  URL

https://api.l2t.io/dz/s/api/v1/sms?fct=dlr&key=%KEY%&msg_id=%MSG_ID%&content-type=%CONTENT-TYPE%

### Paramètres
%KEY%: Votre Clé d’Autorisation.

%MSG_ID%: L’ID du message obtenu précédemment.

%CONTENT-TYPE%: (Facultatif) "JSON" ou "XML".

### Exemple

https://api.l2t.io/dz/s/api/v1/sms?fct=dlr&key=votre_clé_ici&msg_id=12345&content-type=JSON

### Réponse
```json

{
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
```
Signification des États

DELIVERED: Le SMS a été livré.

UNDELIVERED: Non livré.

EXPIRED: Délai expiré.

REJECTED: Refusé par le destinataire.

UNKNOWN: En attente, à vérifier plus tard.

3. Consulter Votre Solde

### 🔗  URL
https://api.l2t.io/dz/s/api/v1/sms?fct=balance&key=%KEY%&content-type=%CONTENT-TYPE%

### Exemple
https://api.l2t.io/dz/s/api/v1/sms?fct=balance&key=votre_clé_ici&content-type=JSON

### Réponse
```json
{
  "success": true,
  "message": "Solde du compte",
  "code": "balance_success",
  "status": 200,
  "data": {
    "id_client": 1234,
    "balance": 500
  }
}
```
### Comprendre la Réponse

success: true → La requête a réussi.

message: "Solde du compte" → Détails du solde.

balance: 500 → Nombre de crédits SMS restants.

4. Récupérer la Liste des Expéditeurs

###  🔗 URL
https://api.l2t.io/dz/s/api/v1/sms?fct=sender&key=%KEY%&content-type=%CONTENT-TYPE%

### Exemple
https://api.l2t.io/dz/s/api/v1/sms?fct=sender&key=votre_clé_ici&content-type=JSON

### Réponse
```json
{
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
```
## Conseils pour Réussir

### 🔒 Protégez Votre Clé
Ne partagez pas votre Clé d’Autorisation publiquement.

### 💻Testez d’Abord
Utilisez un outil comme Postman pour tester les URLs avant de coder.

### 💬 Contactez le Support
En cas de problème ou pour envoyer des SMS longs, contactez le support d’AlgerieSMS.

### 🌍 Utilisez des Numéros Internationaux
Utilisez toujours les codes de pays sans "+" ni "00" (ex.: 21612345678).