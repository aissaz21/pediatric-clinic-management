# Cabinet Medical — Gestion de Cabinet Pédiatrique

## Présentation

Application de gestion complète pour cabinet médical pédiatrique, développée en **Python** avec le framework **Django** et une base de données **SQLite**. L'interface utilisateur est conçue pour fonctionner dans une fenêtre **Microsoft Edge (mode App)**, offrant une expérience moderne et réactive sans dépendance à un navigateur externe.

L'application est entièrement en **français**, optimisée pour un **cabinet médical unipersonnel** (un seul médecin), sans gestion de stock pharmaceutique.

---

## Table des Matières

1. [Fonctionnalités](#fonctionnalités)
2. [Architecture Technique](#architecture-technique)
3. [Structure du Projet](#structure-du-projet)
4. [Installation](#installation)
5. [Configuration](#configuration)
6. [Modules Fonctionnels](#modules-fonctionnels)
7. [Modèle de Données](#modèle-de-données)
8. [Utilisation](#utilisation)
9. [Sauvegarde et Restauration](#sauvegarde-et-restauration)
10. [Sécurité](#sécurité)
11. [Licence](#licence)

---

## Fonctionnalités

### Modules Principaux

| Module | Description |
|--------|-------------|
| **Tableau de Bord** | Vue d'ensemble quotidienne avec statistiques, consultations du jour et alertes |
| **Patients** | Gestion complète des dossiers patients avec historique médical |
| **Salle d'Attente** | Gestion en temps réel de la file d'attente avec priorités |
| **Consultation** | Interface de consultation avec diagnostic et observations |
| **Ordonnance** | Génération et impression de prescriptions médicales personnalisables |
| **Rendez-vous** | Planification et suivi des rendez-vous patients |
| **Documents Médicaux** | Bilans, Certificats, Orientations |
| **Pédiatrie** | Courbes de croissance OMS, vaccinations, données pédiatriques |
| **Finance** | Factures, Honoraires, Dépenses, Suivi des paiements |
| **Rapports** | Rapport de fin de journée + Statistiques globales avec graphiques |
| **Paramètres** | Configuration complète de l'application (12 thèmes) |

### Caractéristiques Clés

- **Interface moderne** : Design responsive adapté aux écrans de bureau (Bootstrap 5).
- **Fonctionnement hors-ligne** : Aucune connexion Internet requise (Chart.js, Bootstrap chargés en local).
- **Base de données embarquée** : SQLite en fichier unique, facilement sauvegardable.
- **Personnalisation complète** : 12 thèmes (Ocean, Nature, Night, Sunset, Lavender, Light, Rainbow, Games, Bear, Candy, Sunny, Magic), modèles d'ordonnance, informations de clinique.
- **Impression intégrée** : Ordonnances (A5, 3 modes), documents et rapports imprimables directement.
- **Recherche avancée** : Recherche patients par nom, téléphone ou numéro de dossier.
- **Autocomplétion** : Saisie assistée pour les médicaments et diagnostics fréquents.
- **Statistiques avancées** : Graphiques interactifs (Chart.js) et rapports imprimables avec comparaison de périodes.
- **Gestion des médicaments** : Base de référence, favoris, dictionnaire personnel du médecin.
- **Mode Desktop** : Fenêtre Edge App avec barre de titre personnalisée.

---

## Architecture Technique

```

┌─────────────────────────────────────────────┐
│        Interface Utilisateur (Edge)         │
│       (HTML5 / CSS3 / JS / Chart.js)        │
├─────────────────────────────────────────────┤
│            Serveur Django (WSGI)            │
│          (URLs / Vues / Templates)          │
├─────────────────────────────────────────────┤
│           ORM Django + SQLite               │
│         (Fichier unique local)              │
└─────────────────────────────────────────────┘

```

### Technologies Utilisées

| Composant | Technologie | Version |
|-----------|-------------|---------|
| Backend | Python | 3.10+ |
| Framework Web | Django | 6.1+ |
| Base de données | SQLite | 3.x |
| ORM | Django ORM | - |
| Frontend | HTML5 / CSS3 / JavaScript | - |
| UI Framework | Bootstrap 5 | 5.x (local) |
| Graphiques | Chart.js | 4.x (local) |
| Navigateur | Microsoft Edge / Google Chrome | - |
| Packaging | PyInstaller + Inno Setup | - |

---

## Structure du Projet

```

cabinet_medical_django/
│
├── manage.py                    ← Outil d'administration Django
├── desktop.py                   ← Point d'entrée (Django + Edge App Mode)
├── init_database.py             ← Initialisation de la base de données
├── build_installer.bat          ← Script de build (PyInstaller + Inno Setup)
├── CabinetMedical.spec          ← Configuration PyInstaller
├── installer.iss                ← Configuration Inno Setup
├── cabinet_config.json          ← Configuration de la clinique (JSON)
├── requirements.txt             ← Dépendances Python
│
├── config/                      ← Configuration Django
│   ├── settings/
│   │   ├── base.py              ← Réglages de base
│   │   ├── dev.py               ← Réglages développement (SQLite)
│   │   ├── desktop.py           ← Réglages mode EXE (SQLite)
│   │   └── prod.py              ← Réglages production (PostgreSQL)
│   ├── urls.py                  ← URLs racine (avec service STATIC/MEDIA en mode EXE)
│   └── wsgi.py
│
├── apps/                        ← Applications Django
│   ├── accounts/                ← Utilisateurs et authentification
│   ├── appointments/            ← Rendez-vous et Salle d'attente
│   ├── audit/                   ← Journal d'activité
│   ├── billing/                 ← Factures, Honoraires, Dépenses
│   ├── consultations/           ← Consultations médicales
│   ├── core/                    ← Paramètres et dashboard
│   ├── documents/               ← Bilans, Certificats, Orientations
│   ├── patients/                ← Gestion des patients
│   ├── pediatric/               ← Pédiatrie (croissance, vaccins)
│   ├── prescriptions/           ← Ordonnances et médicaments
│   └── reports/                 ← ⭐ Rapports et Statistiques
│
├── templates/                   ← Templates Django globaux
│   ├── base.html                ← Template de base (navbar avec Statistiques)
│   ├── 403.html                 ← Erreur 403
│   ├── 404.html                 ← Erreur 404
│   ├── 500.html                 ← Erreur 500
│   └── settings/                ← Templates des paramètres
│       ├── index.html           ← Page principale des paramètres
│       ├── audit_log.html
│       ├── backup_list.html
│       ├── import_*.html        ← Pages d'import JSON
│       ├── restore_pending.html
│       ├── tabs/                ← 16 onglets de paramètres
│       │   ├── cabinet.html, users.html, theme.html
│       │   ├── ordonnance.html, facture.html
│       │   ├── documents.html, _documents_section.html
│       │   ├── medicaments.html, posologies.html
│       │   ├── pediatrie.html, honoraires.html
│       │   ├── bilans.html, backup.html
│       │   ├── certificats.html, orientations.html
│       │   ├── ordonnances_templates.html, apropos.html
│       └── themes/              ← 12 aperçus de thèmes
│           └── ocean.html, nature.html, night.html, etc.
│
├── static/                      ← Fichiers statiques
│   ├── css/                     ← Bootstrap, Icons, Themes
│   ├── js/                      ← Scripts JS
│   ├── vendor/js/               ← Chart.js, Bootstrap Bundle
│   └── images/                  ← Logos et images
│
├── assets/                      ← Ressources (Logo, Icons pour l'EXE)
├── pyinstaller_hooks/           ← Hooks PyInstaller personnalisés
│
├── instance/                    ← (Runtime - Développement)
│   └── clinic.db                ← Base de données SQLite
│
├── media/                       ← (Runtime) Fichiers médias
│   ├── patients/photos/         ← Photos des patients
│   └── patient_images/          ← Radios, échographies, etc.
│
├── logs/                        ← (Runtime) Journaux d'erreurs
├── backups/                     ← (Runtime) Sauvegardes automatiques
│
├── dist/                        ← (Build) Sortie PyInstaller
│   └── CabinetMedical/
│       ├── CabinetMedical.exe
│       ├── _internal/
│       ├── instance/
│       ├── logs/
│       ├── backups/
│       └── ...
│
└── installer_output/            ← (Build) Sortie Inno Setup
└── CabinetMedical_Setup_v3.0.1.exe

```

---

## Installation

### Prérequis

- Python 3.10 ou supérieur
- pip (gestionnaire de paquets Python)
- Système d'exploitation : Windows 10/11
- Microsoft Edge ou Google Chrome installé

### Étapes d'Installation

#### 1. Cloner ou extraire le projet

```bash
cd /chemin/vers/cabinet_medical_django
```

2. Créer un environnement virtuel

```bash
python -m venv venv
venv\Scripts\activate
```

3. Installer les dépendances

```bash
pip install -r requirements.txt
```

Contenu de requirements.txt :

```
Django==6.1
python-decouple==3.8
Pillow==10.1.0
qrcode==7.4.2
pyinstaller==6.10.0
```

4. Initialiser la base de données

```bash
python init_database.py
```

Cette commande crée :

· La structure de la base de données (migrations)
· Un compte administrateur (admin / admin123)
· Les paramètres de clinique par défaut
· Les actes médicaux, vaccins, référentiels OMS, bilans et certificats

5. Lancer l'application

```bash
python desktop.py
```

L'application s'ouvre dans une fenêtre Microsoft Edge (mode App).

---

Configuration

Fichier de Configuration (cabinet_config.json)

```json
{
  "theme": "ocean",
  "cabinet_name": "Dr BOUDJAHFA Abdessamed",
  "cabinet_name_ar": "د. بوجحفة عبد الصمد",
  "medecin": "Dr BOUDJAHFA Abdessamed",
  "specialite_ordonnance": "PEDIATRE",
  "order_number": "6749 / 31",
  "city_ordonnance": "Oran",
  "show_logo": true,
  "show_watermark": true,
  "title_color": "#2c3e50",
  "font_size": 12
}
```

Variables d'Environnement (.env - optionnel)

```env
SECRET_KEY=votre-cle-secrete-tres-longue-et-aleatoire
DB_PATH=D:\Projects_Py\cabinet_medical_django\instance\clinic.db
MEDIA_ROOT=D:\Projects_Py\cabinet_medical_django\media
```

---

Modules Fonctionnels

1. Tableau de Bord (Dashboard)

Accès : Page d'accueil après connexion (core:index)

Contenu :

· Statistiques du jour : patients en attente, consultations réalisées, rendez-vous prévus.
· Graphiques d'activité mensuelle.
· Alertes : rendez-vous à venir, patients en attente prolongée.
· Activité récente : 5 dernières consultations.

---

2. Patients

Fonctionnalités :

· Création de fiche patient complète.
· Recherche avancée (nom, prénom, téléphone, numéro de dossier).
· Historique médical chronologique.
· Informations personnelles, médicales et administratives.
· Contact d'urgence.
· Photo du patient.
· Onglets dédiés : Vue d'ensemble, Croissance (Courbes OMS), Rendez-vous, Consultations, Ordonnances, Vaccinations, Bilans, Orientations, Certificats, Images/Radios, Factures, Honoraires.

---

3. Salle d'Attente

Gestion des statuts :

Statut Description Couleur
EN_ATTENTE Patient en salle d'attente Orange
EN_CONSULTATION Patient avec le médecin Vert
TERMINE Consultation terminée Gris
ANNULE Patient annulé Rouge

---

4. Consultation

Interface divisée en sections :

· Patient & Consultation : Sélection du patient, type, date.
· Signes vitaux : Tension, Température, Poids, Taille, IMC automatique.
· Pédiatrie : Périmètre crânien, Saturation O₂, Fréquences cardiaque/respiratoire.
· Examen & Diagnostic : Plainte, Examen clinique, Diagnostic, Traitement (avec boutons rapides "Ajouter").
· Honoraires : Sélection d'acte pour facturation automatique.

---

5. Ordonnance

Structure personnalisable :

· En-tête : Logo clinique, nom du médecin, adresse, coordonnées.
· Corps : Informations patient, date, liste des médicaments.
· Pied de page : Signature, cachet, mentions légales.

Gestion des médicaments :

· Autocomplétion depuis la liste de référence.
· Modèles d'ordonnance prédéfinis.
· Historique des ordonnances du patient.
· 3 modes d'impression : A5 blanc, A5 pré-imprimé, Ordonnance pré-imprimé.
· Affichage médicaments : Nom commercial, DCI, ou les deux (configurable).

---

6. Pédiatrie

Fonctionnalités :

· Courbes de croissance : Poids, Taille, Périmètre crânien (avec référentiel OMS intégré).
· Vaccinations : Calendrier vaccinal, alertes de retard, saisie des vaccins administrés.
· Données néonatales : Poids naissance, Taille naissance, Apgar, Allaitement.

---

7. Documents Médicaux

· Bilans : Création, saisie de résultats, impression.
· Certificats : Aptitude sportive, scolaire, maladie, vaccination.
· Orientations : Vers spécialistes (Cardiologie, Radiologie, Ophtalmologie...).

---

8. Finance (Facturation)

· Factures : Création, suivi des paiements (payé, partiel, impayé).
· Honoraires : Actes médicaux avec montants automatiques.
· Dépenses : Suivi des charges du cabinet.
· Tableau de bord financier : Vue d'ensemble des revenus et dépenses.

---

9. ⭐ Rapports et Statistiques

Accès : Menu Statistiques (nouveau bouton dans la barre de navigation).

Contenu :

· Filtrage par période : Sélection libre de dates (Du / Au).
· Cartes de synthèse : Patients, Consultations, Ordonnances, RDV, Honoraires, Dépenses, Bénéfice Net.
· Comparaison avec période précédente : Badges de tendance (↑ / ↓).
· Meilleur jour : Jour le plus chargé de la période.
· Graphiques interactifs (Chart.js local) :
  · Évolution financière (Honoraires vs Dépenses).
  · Répartition par sexe (Doughnut).
  · Types de consultations (Pie).
  · Top 5 Diagnostics (Barres horizontales).
  · Top 5 Médicaments (Barres verticales).
  · Pyramide des âges (Doughnuts par tranche d'âge).
· Tableau détaillé des tranches d'âge avec pourcentages.
· Export CSV : Téléchargement d'un rapport complet au format Excel.
· Impression : Bouton Imprimer ouvrant une page A4 optimisée pour l'impression papier.

---

10. Paramètres

10.1 Informations de la Clinique

· Nom de la clinique (FR + AR)
· Adresse complète
· Téléphone, Email
· Numéros d'ordre professionnel
· Logo (upload d'image)

10.2 Configuration de l'Ordonnance

· En-tête, Corps, Pied de page (HTML configurable).
· Prévisualisation en temps réel.

10.3 Liste des Médicaments

· Base de données de référence.
· Import/Export JSON.
· 3 onglets : Tous, Favoris, Mon Dictionnaire.

10.4 Gestion des Utilisateurs

Rôle Permissions
ADMIN Accès complet à toutes les fonctionnalités
MEDECIN Patients, Consultations, Ordonnances, Documents
SECRETAIRE Patients, Rendez-vous, Salle d'attente

10.5 Thèmes

· 12 thèmes prédéfinis : Ocean, Nature, Night, Sunset, Lavender, Light, Rainbow, Games, Bear, Candy, Sunny, Magic.
· Personnalisation des couleurs.

---

Modèle de Données

Applications et Modèles Clés

Application Modèles Principaux
accounts User (utilisateur personnalisé)
patients Patient, Responsable, PatientResponsable, Antecedent, PatientImage, HistoriqueMedical
appointments Appointment, Waiting
consultations Consultation
prescriptions Medication, Posologie, Prescription, PrescriptionDetail, PrescriptionTemplate, PrescriptionTemplateItem, DoctorMedication
billing Acte, Invoice, Payment, Honoraire, Depense, InvoiceLine
documents Bilan, BilanTemplate, Certificat, CertificatTemplate, Orientation, OrientationTemplate
pediatric Enfant, Croissance, CroissanceReference, VaccineCalendar, Vaccination
audit AuditLog
reports (Pas de modèles propres - utilise les autres apps)

---

Utilisation

Premier Démarrage

1. Lancer l'application avec python desktop.py.
2. Se connecter avec les identifiants par défaut :
   · Utilisateur : admin
   · Mot de passe : admin123
3. Important : Modifier immédiatement le mot de passe dans Paramètres > Utilisateurs.
4. Configurer les informations de la clinique et du médecin.
5. Personnaliser le modèle d'ordonnance.
6. Importer ou saisir la liste des médicaments de référence.

Flux de Travail Quotidien

```
08:00 - Ouvrir l'application → Tableau de bord affiche les rendez-vous du jour.
08:30 - Patient arrive → Ajouté à la Salle d'Attente.
08:45 - Médecin appelle le patient → Consultation s'ouvre avec la fiche patient.
09:00 - Rédaction du diagnostic et création de l'ordonnance.
09:15 - Impression de l'ordonnance → Patient terminé.
12:30 - Fin de la matinée → Consultation des Statistiques (avec filtrage par période).
```

Raccourcis Clavier

Raccourci Action
Ctrl + N Nouveau patient
Ctrl + O Nouvelle ordonnance
Ctrl + S Sauvegarder
Ctrl + P Imprimer
Ctrl + F Rechercher
F5 Rafraîchir
F11 Plein écran
Alt + F4 Fermer l'application
Échap Fermer/Annuler

---

Sauvegarde et Restauration

Sauvegarde Automatique

· Une copie de la base de données est créée quotidiennement dans backups/ lors du démarrage ou d'une migration.
· Format du fichier : clinic_auto_YYYYMMDD_HHMMSS.db.
· Conservation des 10 dernières sauvegardes.

Sauvegarde Manuelle

```bash
# Copier le fichier de base de données
copy instance\clinic.db backups\ma-sauvegarde.db
```

Restauration

L'application dispose d'un système de restauration différée :

1. Depuis l'interface (Paramètres > Sauvegardes), téléverser le fichier .db.
2. Le fichier restore_pending.flag est créé.
3. L'application redémarre automatiquement et restaure la base.
4. Une sauvegarde de sécurité de la base actuelle est créée avant l'écrasement.

Attention : La restauration écrase toutes les données actuelles.

---

Sécurité

Authentification

· Mots de passe hashés avec PBKDF2 (Django natif).
· Sessions avec expiration automatique.
· Protection CSRF sur tous les formulaires.

Données Sensibles

· Base de données locale, non exposée sur réseau.
· Pas de transmission sur Internet.
· Serveur Django limité à 127.0.0.1 (localhost).

Bonnes Pratiques

1. Changer le mot de passe par défaut immédiatement.
2. Effectuer des sauvegardes régulières sur support externe (USB).
3. Verrouiller la session lors de l'absence du poste.
4. Ne pas partager les identifiants.
5. Maintenir le système d'exploitation à jour.

---

Dépannage

Problèmes Courants

Problème Solution
L'application ne démarre pas Vérifier que Python 3.10+ est installé et que l'environnement virtuel est activé.
Erreur de base de données Vérifier les permissions d'écriture sur le dossier instance/.
L'interface ne s'affiche pas Vérifier qu'Edge est installé et que le port n'est pas utilisé.
Impression impossible Vérifier la configuration de l'imprimante système.
Mot de passe oublié Supprimer la base et relancer init_database.py (perte de données).

Logs

Les logs d'erreur sont disponibles dans :

· logs/cabinet_medical.log : Logs de l'application (mode EXE).
· logs/errors.log : Erreurs (mode Desktop).

---

Développement

Lancer en Mode Développement

```bash
# Avec Django natif
python manage.py runserver

# Ou avec le script desktop
python desktop.py
```

Créer des Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

Générer l'Installateur

```bash
build_installer.bat
```

Le fichier installer_output\CabinetMedical_Setup_v3.0.1.exe sera généré.

---

Licence

Ce projet est sous licence MIT.

```
Copyright (c) 2026

Permission est accordée, gratuitement, à toute personne obtenant une copie
de ce logiciel et des fichiers de documentation associés, de traiter le
Logiciel sans restriction, y compris, sans limitation, les droits d'utiliser,
de copier, de modifier, de fusionner, de publier, de distribuer, de sous-licencier
et/ou de vendre des copies du Logiciel, et de permettre aux personnes à qui le
Logiciel est fourni de le faire, sous réserve des conditions suivantes :

La notice de copyright ci-dessus et cette permission doivent être incluses dans
toutes les copies ou parties substantielles du Logiciel.

LE LOGICIEL EST FOURNI "TEL QUEL", SANS GARANTIE D'AUCUNE SORTE, EXPLICITE OU
IMPLICITE, Y COMPRIS MAIS SANS S'Y LIMITER LES GARANTIES DE QUALITÉ MARCHANDE,
D'ADÉQUATION À UN USAGE PARTICULIER ET D'ABSENCE DE CONTREFAÇON. EN AUCUN CAS
LES AUTEURS OU TITULAIRES DU COPYRIGHT NE SERONT RESPONSABLES DE TOUTE RÉCLAMATION,
DOMMAGES OU AUTRE RESPONSABILITÉ, QUE CE SOIT DANS UNE ACTION CONTRACTUELLE,
DÉLICTUELLE OU AUTRE, DÉCOULANT DE, HORS DE OU EN RELATION AVEC LE LOGICIEL
OU L'UTILISATION OU AUTRES TRANSACTIONS DANS LE LOGICIEL.
```

---

Contact et Support

Pour toute question, suggestion ou signalement de bug :

· Email : contact@cabinet.dz
· Développeur : Dr BOUDJAHFA Abdessamed

---

Changelog

Version 3.0.1 (2026)

· Rapports : Ajout de la page Statistiques Globales avec :
  · Filtrage par période.
  · Comparaison avec période précédente.
  · Graphiques interactifs (Chart.js local).
  · Export CSV.
  · Impression papier (A4).
· Pédiatrie : Courbes de croissance OMS avec référentiel intégré.
· Finance : Suivi des paiements et dépenses.
· Documents : Bilans, Certificats, Orientations.
· Sécurité : Sessions et protection CSRF.
· Performance : Optimisation des requêtes (select_related, prefetch_related).
· Interface : Nouveau design responsive avec Bootstrap 5 (local).

Version 1.0.0 (2024)

· Version initiale (Flask) - Remplacée par Django en 2026.

---

Document généré le 16 novembre 2026
Application Cabinet Medical v3.0.1 
