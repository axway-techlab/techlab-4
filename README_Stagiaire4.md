# Techlab - Faciliter le partage de données en toute sécurité
# Liens Utiles

- [Amplify platform](https://platform.axway.com ) pour ce cas d'usage
- [SecureTransport WebClient](https://ptx140.demo.axway.com:8443)

# Sommaire
- [Cas d'usage](#cas-usage)
  * [Distribution d'ordonnance à travers du transfert de fichier](#description)
  * [Les Etapes](#les-etapes)
- [Création du Flux](#create-flux)
- [Exécution du Transfert](#execute-transfer)
- [Recuperation du fichier](#get-file)
   
## Cas d'usage

### Distribution d'ordonnance à travers du transfert de fichier
A travers la plateforme Amplify nous allons utiliser les APIs des Outils de Transferts de fichiers d'Axway MFT.
Ils vont permettre de rapidement mettre à disposition de Pharmacies des Ordonnances de patients.

### Les Etapes

Outre la découverte de la plateforme dans son ensemble voici les différentes étapes qui vont être décrites ici : 
- Création du flux sur les différents composants d'infrastructures. **FLOW MANAGER APIs**
![image](https://user-images.githubusercontent.com/78549144/136689486-0135ab69-0e30-44f9-8cf7-85a74205d839.png)
- Exécution d'un Transfert d'ordonnance. **TRANSFER CFT APIs**
- Récupération de l'ordonnance. **SECURETRANSPORT WEB CLIENT**

## Création du Flux

Pour cela on va utiliser l'APIs de FLow Manager à travers la méthode `subscriptions` : 
Le partenaire `PHARMACIE4` a déjà été pré-crée pour les besoins du techlab, il devra se connecter en HTTP afin de récupérer
les ordonnances mises à sa disposition.

Voici le json qui sera a fournir en body de la requête 

```json
{  
"name": "Publication Ordonnance a PHARMACIE4",
"templateName": "ORDONNANCE",
"flowName": "Flux Ordonnance a PHARMACIE4",
"type": "Subscription",
"participants": [
  {
    "businessId": "?name=PHARMACIE4",
    "comProfileId": "?name=HTTP-4",
    "role": "target",
    "context": {
      "participantId": "?name=PHARMACIE4",
      "participantType": "PARTNER",
      "participantProfileId": "?name=HTTP-4",
      "protocol": "HTTP",
      "protocolIndex": 1,
      "relaysSize": 0,
      "direction": "RECEIVER_PULLS_FILE",
      "protocolProperties": {
        "httpMethodsCheck": "NO",
        "httpTransferMode": "AUTODETECT",
        "httpMethodsInput": "PUT,GET,POST",
        "securityProfile": "SERVER_ONLY"
      }
    }
  }
]
}
```

Le flux a été crée

## Exécution du Transfert

Pour cela on va utiliser l'API de TransferCFT à travers la méthode `transfers` :  
Voici les paramètres a renseigner :
  - part : `SECURETRANSPORT`
  - idf  : `IDF_TECHLAB_PHARMACIE4`

Body de la requête :

```json
{
  "fname": "/tmp/techlab/ordonnance_stagiaire4.pdf",
  "nfname": "ordonnance_stagiaire4.pdf"
}
```

Le Transfert a été executé

## Récupération du fichier

Pour cela on va utiliser le client Web intégré à l'outil Axway SECURETransport 

Se connecter à : [ST Web Client](https://ptx140.demo.axway.com:8443)

- UserName : `user_pharm4`
- Password : _Il vous sera donné par le formateur_

Dans le répertoire TECHLAB vous devrez retrouver l'ordonnance envoyée avec le moniteur de Transfet de fichier TransferCFT.

Merci.
