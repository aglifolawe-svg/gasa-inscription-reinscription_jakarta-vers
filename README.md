# 🏛 GASA Formation — Système de Gestion des Inscriptions et Réinscriptions Universitaires

> Plateforme de gestion numérique des dossiers d'inscription universitaire pour UATM GASA Formation, couvrant les cycles Licence 1 à Master 2.

---

## 📌 Contexte

Dans le cadre de la transformation numérique des processus administratifs de GASA Formation, ce projet vise à remplacer les procédures manuelles d'inscription par une plateforme web robuste, traçable et entièrement gérée par les équipes du secrétariat.

Le système prend en charge :
- Les **nouveaux entrants en Licence 1**
- Les **réinscriptions de Licence 2 à Master 2**
- La **validation des dossiers** par le secrétariat via un dashboard dédié

---

## ✨ Fonctionnalités

### Côté Étudiant
- Formulaire d'inscription dynamique et réactif
- Affichage automatique des matières selon la série du BAC sélectionnée
- Upload des pièces justificatives (photo, extrait de naissance, relevés de notes, attestation BAC)
- Pré-validation automatique (compatibilité série/filière, moyenne, fichiers)
- Suivi en temps réel de l'état de son dossier
- Notifications à chaque changement d'état

### Côté Secrétariat (Dashboard)
- Vue d'ensemble de tous les dossiers soumis (filtres, pagination, tri)
- Traitement individuel ou par lot (confirmer / rejeter / demander complément)
- Visualisation complète du dossier + téléchargement des pièces
- Commentaires personnalisés sur chaque dossier
- Export PDF / Excel des listes d'étudiants
- Audit trail complet de toutes les actions

---

## 🏗 Architecture Technique

Ce projet a été développé en **deux versions** progressives :

### Version 1 — JSP + JDBC (Architecture MVC)
- Couche présentation : **JavaServer Pages (JSP)**
- Couche accès données : **JDBC natif** avec pattern **DAO**
- Configuration manuelle des connexions et requêtes SQL
- Déploiement sur serveur **GlassFish**

### Version 2 — EJB + JSF (Architecture composants réutilisables)
- Couche présentation : **JavaServer Faces (JSF)**
- Couche métier : **Enterprise JavaBeans (EJB)** — composants indépendants et réutilisables
- Persistance : **JPA (Java Persistence API)**
- Génération de rapports : **JasperReport**
- Déploiement : **GlassFish Application Server**

> L'objectif de la v2 était d'obtenir des composants métier découplés, capables de s'intégrer et d'être réutilisés dans d'autres contextes sans modification.

---

## 🗄 Modèle de Données (Entités principales)

| Entité | Rôle |
|---|---|
| `Etudiant` | Profil de l'étudiant (nom, prénom, email, téléphone…) |
| `InscriptionDossier` | Dossier d'inscription (niveau, filière, état, dates) |
| `Justificatif` | Pièces justificatives uploadées (type, fichier, validité) |
| `BacSerie` | Série du BAC et matières principales associées |
| `MatierePrincipale` | Matières et coefficients par série |
| `Filiere` | Filières disponibles avec critères d'admission |
| `NoteBac` | Notes par matière pour un dossier donné |
| `AuditTrail` | Historique de toutes les actions (user, timestamp, action) |
| `Notification` | Notifications persistantes (type, état lu/non lu) |

---

## ⚙️ Règles de Pré-validation Automatique

Avant soumission au secrétariat, le système vérifie automatiquement :

1. **Compatibilité série BAC / filière** — blocage si incompatible
2. **Moyenne et seuils** — calcul pondéré, comparaison au seuil de la filière
3. **Pièces justificatives** — présence, format (PDF/JPG), poids max respecté
4. **Unicité** — un étudiant ne peut pas soumettre deux dossiers simultanés sur la même filière
5. **Consentements RGPD** — validation requise pour activer la soumission

---

## 🛠 Stack Technique

| Couche | Technologies |
|---|---|
| Présentation | JSP (v1) → JSF (v2) |
| Métier | EJB (Session Beans, Message-Driven Beans) |
| Persistance | JDBC (v1) → JPA / Hibernate (v2) |
| Rapports | JasperReport |
| Serveur | GlassFish Application Server |
| Base de données | MySQL / PostgreSQL |
| Langage | Java (Jakarta EE) |

---

## 📁 Structure du Projet

```
gasa-inscription/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── model/          # Entités JPA
│   │   │   ├── dao/            # Couche accès données (DAO pattern)
│   │   │   ├── ejb/            # EJBs métier
│   │   │   ├── service/        # Services (notification, audit, dashboard)
│   │   │   └── controller/     # Managed Beans JSF
│   │   ├── resources/
│   │   │   └── META-INF/
│   │   │       └── persistence.xml  # Config JPA
│   │   └── webapp/
│   │       ├── WEB-INF/
│   │       │   └── web.xml
│   │       ├── pages/          # Vues JSF (.xhtml)
│   │       └── reports/        # Templates JasperReport (.jrxml)
└── pom.xml
```

---

## 🚀 Installation & Démarrage

### Prérequis
- JDK 11+
- GlassFish Application Server 5+
- Maven
- Base de données (MySQL ou PostgreSQL)

### Étapes

```bash
# 1. Cloner le projet
git clone https://github.com/aglifolawe-svg/gasa-inscription.git
cd gasa-inscription

# 2. Configurer la base de données dans persistence.xml
# Modifier : url, username, password

# 3. Compiler et packager
mvn clean package

# 4. Déployer sur GlassFish
# Copier le .war généré dans le dossier autodeploy de GlassFish
# ou utiliser la console d'administration GlassFish (port 4848)

# 5. Accéder à l'application
# http://localhost:8080/gasa-inscription
```

---

## 📊 Génération de Rapports

Les rapports PDF (listes d'inscrits, récapitulatifs de dossiers) sont générés via **JasperReport**.  
Les templates `.jrxml` se trouvent dans `src/main/webapp/reports/`.

---

## 👨‍💻 Auteur

**Folawè Milarépa AGLI**  
Étudiant en Licence Professionnelle SIL — UATM GASA Formation  
[github.com/aglifolawe-svg](https://github.com/aglifolawe-svg)

---

## 📄 Licence

Projet académique — UATM GASA Formation.  
Libre d'utilisation à des fins d'apprentissage.
