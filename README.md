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

**Attention :** il y a un décalage entre le dépôt distant et le Config Server. Celui-ci cherche les fichiers dans un sous-dossier `config-repo/`, alors qu'ils sont actuellement à la racine du dépôt. Pour servir la structure actuelle, régler `spring.cloud.config.server.git.search-paths` à `.` (ou supprimer cette propriété). L'autre option est de déplacer les fichiers `.properties` dans un sous-dossier `config-repo/`.

Le README du projet Billing mentionne également une autre URL pour le dépôt de configuration (`billing-microservices.git`). Harmoniser cette documentation avec l'URL réellement utilisée par le Config Server.

Une fois le chemin corrigé, les configurations peuvent être vérifiées, par exemple à `http://localhost:8888/service-clients/default`.

## Modifier une configuration

1. Modifier le fichier `.properties` de l'application concernée.
2. Vérifier que la syntaxe et les noms de propriétés sont corrects.
3. Commiter et pousser les changements vers la branche et le dépôt utilisés par le Config Server.
4. Vérifier la configuration servie, par exemple avec `http://localhost:8888/service-clients/default`.
5. Redémarrer le service concerné pour qu'il recharge sa configuration, sauf si un mécanisme de rafraîchissement a été configuré explicitement.

Le `refresh-rate=60` du Config Server régit la fréquence de mise à jour de son clone Git ; il ne garantit pas, à lui seul, le rechargement à chaud d'une application déjà démarrée.
