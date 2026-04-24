# 📑 Rapport de POC : Conteneurisation de l'Application Pozos

## 1. Contexte du Projet
Ce projet s'inscrit dans le cadre de la modernisation de l'infrastructure de **CCNTechnologies**. L'objectif était de transformer une application de gestion d'étudiants en une architecture micro-services conteneurisée, garantissant agilité, portabilité et sécurité.



---

## 2. Architecture Technique
L'infrastructure repose sur quatre composants principaux orchestrés via **Docker Compose** :

* **API (Backend)** : Une application Python Flask sécurisée par authentification HTTP Basic.
* **Website (Frontend)** : Une interface PHP interactive qui consomme l'API.
* **Docker Registry** : Un registre privé local pour le stockage sécurisé des images.
* **Registry UI** : Une interface web pour la gestion et la visualisation des images poussées.

### Flux de données
1. L'utilisateur accède au **Website** (Port 8080).
2. Le **Website** communique avec l'**API** via le réseau interne Docker `student-list-network`.
3. L'**API** interroge le fichier JSON monté via un **Volume**.

---

## 3. Détails d'Implémentation

### Dockerfile (API)
Le choix de l'image de base s'est porté sur `python:3.11-slim` pour sa légèreté.
* **Sécurité** : Installation des dépendances système nécessaires (`python3-dev`, `libsasl2-dev`, etc.).
* **Volumes** : Le fichier `student_age.json` est monté dynamiquement pour permettre des mises à jour de données sans reconstruire l'image.

### Docker Compose
Le fichier `docker-compose.yml` définit les limites de ressources, les réseaux isolés et les variables d'environnement (`USERNAME`, `PASSWORD`) pour la connexion sécurisée entre les services.

---

## 4. Guide d'Installation et Exécution

### Prérequis
* Docker & Docker Compose installés.
* Port 5000, 8080, 5001 et 8081 disponibles.

### Lancement de l'infrastructure
```bash
# Construction et lancement des services
docker compose up --build -d
```

### Déploiement dans le registre privé
```bash
# Tag de l'image
docker tag student-list_api:v1 localhost:5001/student-list_api:v1

# Push vers le registre local
docker push localhost:5001/student-list_api:v1
```

---

## 5. Validation du POC

### Matrice des Ports
| Service | Port Externe | Usage |
| :--- | :--- | :--- |
| **Website** | `8080` | Interface utilisateur finale |
| **API** | `5000` | Endpoint REST (Backend) |
| **Registry** | `5001` | Serveur de stockage d'images |
| **Registry UI** | `8081` | Visualisation du registre |

### Tests de connectivité
Test de l'API via PowerShell :
```powershell
$headers = @{ Authorization = "Basic " + [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes("toto:python")) }
Invoke-RestMethod -Uri "http://localhost:5000/pozos/api/v1.0/get_student_ages" -Method Get -Headers $headers
```

---

## 6. Captures d'Écran (Preuves de Succès)

> **Note :** Les captures ci-dessous valident le bon fonctionnement de l'infrastructure.

1.  **Statut des Conteneurs** 
2.  **Interface Web Fonctionnelle** 
3.  **Registre Privé (Image poussée)** 

---

## 7. Auteur
**FODING KAMDOM ANGE MICHELE**
*Étudiante en 4ème année cycle Ingénieur en Génie Logiciel*
