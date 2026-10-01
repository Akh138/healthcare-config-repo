# ⚙️ Healthcare - Centralized Configuration Repository

![Spring Cloud](https://img.shields.io/badge/Spring_Cloud-Config-blue?style=for-the-badge&logo=spring&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-NoSQL-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Consul](https://img.shields.io/badge/HashiCorp-Consul-E03875?style=for-the-badge&logo=consul&logoColor=white)

Répertoire de **configuration centralisé et externalisé** de l'écosystème médical distribué **Healthcare**.

Ce dépôt est consommé dynamiquement au démarrage de l'infrastructure par le microservice **Spring Cloud Config Server** (port `8888`) pour injecter les paramètres réseau et les chaînes de connexion de chaque service métier.

---

## 🎯 Rôle dans l'Architecture

Conformément au **Facteur III de la méthodologie 12-Factor App (Configuration)**, la configuration applicative est strictement découplée du code source Java et des images Docker :

```text
  [ Dépôt GitHub : healthcare-config-repo ]
                     │
                     ▼ (Pull Git au boot)
        [ Config Server : 8888 ]
                     │
       ┌─────────────┼─────────────┬─────────────┐
       ▼             ▼             ▼             ▼
[ healthcare-ui ] [ note-service ] [ patient-service ] [ user-service ]
  (Port 8080)       (Port 8083)       (Port 8081)         (Port 8082)
       │                 │                 │                   │
   OpenFeign         MongoDB NoSQL       MySQL 8.0           MySQL 8.0
   Timeouts          healthcare_notes   healthcare_db       healthcare_db
```

### Avantages d'Ingénierie :
* **Découplage Environnemental** : Modification des paramètres de base de données, timeouts OpenFeign ou sondes sans recompiler le code Java ni reconstruire les conteneurs Docker.
* **Gestion de la Persistance Polyglotte** : Distribution unifiée des configurations relationnelles (MySQL 8.0) pour les entités administratives et documentaires (MongoDB) pour les notes médicales.
* **Traçabilité & Versioning** : Chaque évolution d'infrastructure est versionnée et historisée via les commits Git.

---

## 📁 Inventaire des Fichiers de Configuration

| Fichier | Service Cible | Moteur de Données | Paramètres Clés Gérés |
| :--- | :--- | :--- | :--- |
| **`healthcare-ui.properties`** | Portail Web Clinique | *Aucun (Front)* | Port `8080`, Consul Discovery, Timeouts OpenFeign (5000 ms) |
| **`note-service.properties`** | Observations Médicales | **MongoDB NoSQL** | Base `healthcare_notes` (27017), Consul Discovery, Actuator |
| **`patient-service.properties`** | Dossiers Patients | **MySQL 8.0** | Base `healthcare_db` (3306), Hibernate DDL `update`, Consul |
| **`user-service.properties`** | Praticiens & Authentification | **MySQL 8.0** | Base `healthcare_db` (3306), Pilote JDBC, Consul Discovery |

---

## 🔗 Projet Principal

L'ensemble du code source des microservices, l'orchestration Docker Compose et l'interface utilisateur Glassmorphism sont disponibles sur le dépôt principal :

👉 **[Healthcare Project (Dépôt Principal)](https://github.com/Akh138/healthcare-project)**
