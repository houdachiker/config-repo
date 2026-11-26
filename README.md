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



