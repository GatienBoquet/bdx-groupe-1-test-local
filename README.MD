
![Logo](https://github.blog/wp-content/uploads/2019/08/DL-V2-LinkedIn_FB.png?fit=1200%2C630)


## 📍 Introduction

Nous avons décidé de protéger la brache main du projet github. 
Afin de réduire la redondance et de faciliter la pipeline CICD.
L'idée est simple : La branche main est prôtégée.

1. Nous avons besoin de faire un pull request avant de mergé sur la 'main'.
2. 2 personnes doivent review la pull request avant de pouvoir mergé sur la 'main'.

Cela assurer une qualité de code et de fonctionnement du projet.

Aussi, nous pouvons ajouter une condition sur les tests de la pipeline CI.
En effet, sur chaque pull request et sur chaque commit de cette pull request, la pipeline CI nous permets de vérifier le bon fonctionnement du projet.

Avec ces trois conditions, nous nous assurons un bon fonctionnement du projet avant de merger sur la main branche.
La branche main représente donc pour nous l'environnement  staging et prod.

Le concept de CICD est respecté et permet un cycle de développement de l'application rapide et concis.

![Dev Ops](https://3.bp.blogspot.com/-Ecj2B__l7nA/XH-X3IULJYI/AAAAAAAABjQ/EtAL40fOPbk_G_t0GfKniuSjZB-lR0HWACLcBGAs/s640/traditional-devops.png)
## 🧮 Les fonctionnallités de la pipeline
Nous avons 3 pipeline, Dev, Staging puis Prod

### ⛹️‍♂️ La pipeline CI Dev, étape par étape

- On récupère le contenu de notre repository
- On installe Node.JS 
- On vérifie le contenu du cache
- On installe les dépendances
- On lance le serveur Node.JS
- On lance les tests unitaires
Puis si cela fonctionne on passe à la sucurité :
- On lance snyk vérifié s'il y a d'éventuelles vulnérabilités
- On upload le résultat sur CodeScanning, le service disponible sur GitHub
S'il y a aucune vulnerabilités, on passe à l'étape du Linter
- On récupère le contenu du repository.
- On vérifie le repo avec super-linter.
Puis un passe à l'étape du build de l'image
- On build l'image avec BuildX
- On la push sur le package repository associé à notre répo github.
Puis pour finir, la dernière étape consiste à notifier du résultat sur slack.

Une fois les test finit, on regarde ensemble pour mérger la pull request.
Si la pull request est mergé, la pipepine staging s'enclenche.

### 🤾‍♂️ La pipeline CD Dev

- On récupère le package sur le repo github 
- On le deploy sur heroku
- On fait un healthcheck pour s'assurer du bon fonctionnement
- On envoie une notification sur Slack

### 🌠 La pipeline CI et CD Staging et Prod

- TODO
- add moniroting


## 🔬 Installation pipeline

- Télécharger les fichiers ci.yml et cd.yml 
- Assurez-vous d'avoir les bonnes variables d'environements
- Avoir un compte AWS et Heroku

### Pipeline Prod
![Pipeline prod](https://i.imgur.com/saUIvmh.png)


![Pipeline prod](https://i.imgur.com/e07mQfIl.png)


## 🖥️ Le stack du site web

**Client:** HTML, CSS

**Serveur:** Node, Express


## ♾️ Le stack de la pipeline

**Conteneurisation:** Docker

**Hébergement:** Heroku et AWS

**Sécurité:** Snik

**Linter:** Super Linter

**Registre de container:** GitHub Registry container

**WebHook:** Slack
## 🔀 Diagramme séquentielle


```mermaid
sequenceDiagram
    autonumber
    actor Developpeur
    participant Repository
    participant CI 
    participant CD
    participant Environements
    participant Slack
    Developpeur->>+Repository: Commit Code 
    loop Every merge request
        Repository->>+CI: Télécharge le repo
        CI->>+CI: Test Unitaires
        CI->>+CI: Test Sécurité
        CI-->>+Repository: Upload du résultat sécurité sur CodeScanning
        CI->>+CI: Super Linter
        CI->>+CI: Build de l'image
        CI->>+Slack: Notification de statut du build
        CI->>+Repository: Upload de l'image sur le repo GitHub
        Repository->>+CD: Téléchargment de la dernière image 
        CD->>+Environements: Déploiement
        Note right of Environements: Heroku (Dev)
        CD-->>CD: Healthcheck 
        CD->>+Slack: Notification de statut du déploiement
        CD->>Repository: Feedback des tests
    end
```

## 🪛 Pour installer le projet

Vous pouvrz installer le projet avec le gestionnaire de paquage npm.
Vous avez besoin d'un serveur Node.JS avec la version 14.16

```bash
  npm install bdx-groupe-1
  cd my-project
```

- [ ]  Avoir un serveur Node.JS
- [ ]  Avoir les variables d'environements en place
- [ ]  Avoir un compte Heroku et AWS
## 📝 Auteurs

- [@arnaudtron](https://www.github.com/arnaudtron)
- [@karim](https://www.github.com/styldown)
- [@gatienboquet](https://www.github.com/gatienboquet)
## 🔔 Badges

Add badges from somewhere like: [shields.io](https://shields.io/)

[![MIT License](https://img.shields.io/apm/l/atomic-design-ui.svg?)](https://github.com/tterb/atomic-design-ui/blob/master/LICENSEs)
[![GPLv3 License](https://img.shields.io/badge/License-GPL%20v3-yellow.svg)](https://opensource.org/licenses/)
[![AGPL License](https://img.shields.io/badge/license-AGPL-blue.svg)](http://www.gnu.org/licenses/agpl-3.0)

## 🔐 Les variables d'environements 

Pour pouvoir assurer le fonctionnement  de la pipeline CICD, vous avez besoin de définir certaines variables. 
Ces variables sont confidentielles, assurez-vous de ne pas les divulguer publiquement.

`HEROKU_API_KEY` : La clé API pour déployer sur Heroku

`SNYK_TOKEN` : La clé pour l'analyse du code avec Snyk

`SLACK_WEBHOOK_URL` : Le lien du webhook Slack

`AWS_PRIVATE_KEY` : La clé privé du serveur EC2 d'AWS

`HOSTNAME` : Le nom d'hote du serveur EC2 d'AWS

`USER_NAME`: L'username du serveur EC2 d'AWS
## 📚 Leçons apprises

What did you learn while building this project? What challenges did you face and how did you overcome them?


## 👏 Remerciements

 - [Ulrich MONJI](https://github.com/ulrichmonji)
 - [Awesome README](https://github.com/matiassingers/awesome-readme)
 - [How to write a Good readme](https://bulldogjob.com/news/449-how-to-write-a-good-readme-for-your-github-project)


## 📜 License

[MIT](https://choosealicense.com/licenses/mit/)


## 💡 Améliorations possibles

