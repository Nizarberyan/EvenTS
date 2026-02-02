# 📄 Contexte du Projet

### Situation Actuelle
Une organisation (centre de formation, entreprise, association ou espace de coworking) organise régulièrement des événements (formations, ateliers, conférences, réunions internes). Actuellement, la gestion des événements et des inscriptions est réalisée de manière essentiellement manuelle (tableurs Excel, formulaires simples, échanges par e-mail ou messagerie).

**Problématiques identifiées :**
* Manque de visibilité en temps réel sur les événements disponibles et les places restantes.
* Erreurs de réservation (doublons, surbooking).
* Difficultés à suivre l’état des réservations (en attente, confirmée, annulée).
* Gestion peu fiable des droits (qui crée l'événement ? qui valide ?).
* Absence de centralisation des informations (participants et historique).

### 🎯 Objectif
Concevoir et développer une application web permettant de gérer des événements et leurs réservations, avec une gestion rigoureuse des **rôles**, de la **sécurité**, de la **qualité logicielle** et de l’**industrialisation**.

---

# 🛠 Fonctionnalités Principales

L'application doit permettre de :
* Créer, modifier, publier et annuler des événements.
* Afficher un catalogue public des événements disponibles.
* Gérer les capacités et le nombre de places restantes.
* Permettre aux utilisateurs de réserver une place.
* Gérer le cycle de vie des réservations (demande, confirmation, refus, annulation).
* Centraliser les informations liées aux événements et aux participants.
* Générer et télécharger un ticket PDF / confirmation PDF pour les réservations confirmées.

---

# 👥 Rôles et Permissions

### 1. Admin
* **Gestion des événements :** Crée, modifie, publie et annule des événements.
    * Définit les attributs : *titre, description, date/heure, lieu, capacité maximale*.
* **Gestion des réservations :**
    * Consulte la liste complète (par événement ou par participant).
    * Confirme, refuse ou annule une réservation (même confirmée).
* **Dashboard :** Accède aux indicateurs clés :
    * Événements à venir.
    * Taux de remplissage.
    * Répartition des réservations par statut.

### 2. Participant
* **Catalogue :** Consulte la liste des événements publiés et leurs détails.
* **Réservation :** Effectue une réservation (si les règles sont respectées).
* **Espace personnel :**
    * Consulte la liste de ses réservations.
    * Annule sa propre réservation.
    * Télécharge son ticket PDF (uniquement si statut `CONFIRMED`).

---

# 📅 Planification JIRA (OBLIGATOIRE)

La planification fait partie intégrante de l’évaluation.

1.  **Création du projet :** Projet JIRA dédié.
2.  **Structure :**
    * **Epics** (ex: Authentification, Gestion des événements, Front-end, CI/CD, etc.).
    * **User Stories / Tasks**.
    * **Sub-tasks**.
3.  **Intégration GitHub :** Référencer les tickets dans les commits (ex: `SC2-15`).
4.  **Automatisation :** Mettre en place au moins une règle (ex: Passage en *Done* lors du merge d'une PR).
5.  **Soutenance :** Être capable d’expliquer la planification et l’avancement.

---

# 💻 Stack Technique

## 🟢 Partie Back-end (NestJS)
* **Langage/Framework :** NestJS (TypeScript).
* **Base de données :** MongoDB ou PostgreSQL.

### Exigences Techniques
* Architecture modulaire (Modules, Controllers, Services).
* Utilisation des **DTO** et validation (`class-validator`).
* Authentification sécurisée via **JWT**.
* Système d’autorisation **RBAC** (Admin / Participant).
* Protection des routes sensibles.
* Gestion centralisée des erreurs (Global Error Handling) et codes HTTP corrects.

### Règles Métier
* **Statuts d'événement :** `DRAFT`, `PUBLISHED`, `CANCELED`.
    * *Seuls les `PUBLISHED` sont visibles publiquement.*
* **Statuts de réservation :** `PENDING`, `CONFIRMED`, `REFUSED`, `CANCELED`.
* **Contraintes de réservation :**
    * Impossible de réserver si : événement annulé/non publié, événement complet, ou déjà réservé par l'utilisateur.
    * La capacité maximale ne doit jamais être dépassée.
    * Le PDF n'est téléchargeable que si le statut est `CONFIRMED`.

### Tests Back-end (OBLIGATOIRES)
* **Tests unitaires (Jest) :** Services critiques (événements, réservations, auth).
* **Tests E2E :** Scénario complet avec rôles distincts.

## 🔵 Partie Front-end (Next.js)
* **Framework :** Next.js + TypeScript.

### Exigences Techniques
* **Rendu hybride :**
    * **SSR :** Pages publiques (Catalogue, Détails).
    * **CSR :** Espaces authentifiés (Dashboards).
* Routing dynamique (`/events/[id]`).
* Protection des routes selon le rôle.
* Communication sécurisée API (JWT).
* **Gestion d’état :** Redux ou Context API.
* Gestion des formulaires et validation client.

### Tests Front-end (OBLIGATOIRES)
* Tests de composants (**React Testing Library**).
* Tests d’un flux fonctionnel (ex: réservation ou annulation).

---

# 🚀 Déploiement & DevOps

## Docker
* Création d'images pour : Front-end, Back-end, Base de données.
* Réseau Docker pour la communication inter-services.
* Fichier `docker-compose.yml` fonctionnel.
* **Variables d'environnement :**
    * Fichier `.env.example` obligatoire.
    * Séparation Dev / Prod documentée.

## CI/CD (GitHub Actions) — OBLIGATOIRE
Pipeline fonctionnelle déclenchée à chaque `push` et `pull_request`.

**Jobs obligatoires (Front + Back) :**
1.  Install / Cache
2.  Lint
3.  Tests
4.  Build

**Critères de réussite :** La pipeline échoue si le lint, les tests ou le build échouent.
**Publication :** Push des images sur **Docker Hub**.