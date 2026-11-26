# Configuration centralisee

Ce dossier contient les fichiers de configuration Spring des applications Billing. Le Config Server les distribue aux services selon leur nom `spring.application.name`.

## Fichiers

| Fichier | Application | Contenu principal |
|---|---|---|
| `gateway-service.properties` | `gateway-service` | Port de la gateway, Eureka et routage des API |
| `service-clients.properties` | `service-clients` | Base clients, Eureka et résilience des appels Feign |
| `service-abonnements.properties` | `service-abonnements` | Base abonnements, Eureka et résilience des appels Feign |
| `service-facturation.properties` | `service-facturation` | Base facturation, TVA, RabbitMQ, Eureka et résilience |
| `notification-service.properties` | `notification-service` | Base notifications, RabbitMQ, SMTP et Eureka |

Le nom du fichier doit correspondre exactement à `spring.application.name` de l'application qui le charge.

## Chargement par le Config Server

Le dépôt distant `https://github.com/houdachiker/config-repo.git` contient actuellement les cinq fichiers `.properties` à sa racine. La configuration locale du Config Server indique :

- dépôt Git distant : `https://github.com/houdachiker/config-repo.git` ;
- branche : `main` ;
- chemin de recherche : `config-repo` ;
- port du Config Server : `8888`.


## Modifier une configuration

1. Modifier le fichier `.properties` de l'application concernée.
2. Vérifier que la syntaxe et les noms de propriétés sont corrects.
3. Commiter et pousser les changements vers la branche et le dépôt utilisés par le Config Server.
4. Vérifier la configuration servie, par exemple avec `http://localhost:8888/service-clients/default`.
5. Redémarrer le service concerné pour qu'il recharge sa configuration, sauf si un mécanisme de rafraîchissement a été configuré explicitement.

Le `refresh-rate=60` du Config Server régit la fréquence de mise à jour de son clone Git ; il ne garantit pas, à lui seul, le rechargement à chaud d'une application déjà démarrée.

## Secrets et securite

- Ne jamais commiter de mot de passe, jeton, clé privée ou certificat privé.
- Référencer les secrets par variables d'environnement, par exemple `${DB_CLIENTS_PASSWORD}`, puis les fournir hors du dépôt.
- Garder dans Git les valeurs non sensibles partagées par l'équipe, ainsi que des exemples avec des valeurs fictives.
- Les valeurs RabbitMQ `guest/guest` présentes dans certaines configurations sont réservées au développement local. Utiliser des identifiants dédiés et injectés hors du dépôt dans tout environnement partagé ou de production.
- Ne pas ignorer tout `config-repo/` : ces fichiers sont nécessaires au Config Server. Seuls les fichiers locaux contenant de vrais secrets doivent être exclus du suivi Git.
