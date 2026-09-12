# University Board Game Library

Full-stack web application designed to organize, search and manage a university collection of more than **17,000 board games**, including historical games dating back to the 19th century.

Developed as a collaborative academic project at **Université Sorbonne Paris Nord**, the application combines a **PHP MVC web application**, a **MySQL relational database** and **Python data-processing tools**.

[View the full case study on my portfolio](https://lasrybeskiwin.fr/work/university-board-game-library/)

![University Board Game Library preview](archive/App/assets/images/site-demo2.gif)

---

## Overview

Université Sorbonne Paris Nord maintains a large board-game collection containing more than **17,000 items**.

The original data was spread across heterogeneous spreadsheet files. The project therefore involved more than building a web interface: the data first had to be cleaned, normalized and reorganized into a relational model that could support real application workflows.

The final system provides tools for:

- browsing and searching the collection
- managing games and physical copies
- tracking physical storage locations
- managing loans and returns
- maintaining historical records
- handling several user roles
- administering users and inventory
- exporting collection data to Excel

---

## Key Features

### Collection management

- Catalogue of **17,000+ board games**
- Multi-criteria search
- Detailed game information
- Authors, publishers, categories and mechanisms
- Physical copy management
- Storage room and shelf tracking
- Game condition tracking
- Barcode information
- Collection provenance

### Loans

- Loan registration and tracking
- Return management
- Availability tracking
- Borrower information
- Loan history
- Administrative loan management

### Authentication and accounts

The application includes account creation, authentication and profile management.

Authentication features include:

- email validation
- password strength validation
- password hashing with **Argon2id**
- session-based authentication
- password verification and updates
- editable account information

Three application roles are represented in the data model:

```text
Admin
Gestionnaire
Utilisateur
```

### Administration

Administrative interfaces provide access to:

- user management
- inventory management
- loan management
- collection search
- historical records
- data export

### Excel export

Administrators can generate an Excel export of the collection.

The PHP application invokes a Python export script which:

1. connects to MySQL
2. queries the relational dataset
3. reconstructs collection information
4. generates an `.xlsx` file
5. returns the generated file through the web application

---

## Data Pipeline

One of the main technical challenges was transforming the original spreadsheet-based inventory into structured relational data.

```text
Excel / source data
        │
        ▼
Python
Pandas / OpenPyXL
        │
        ▼
Cleaning & normalization
        │
        ▼
Structured CSV data
        │
        ▼
MySQL import
        │
        ▼
Relational data model
        │
        ▼
PHP MVC application
```

The Python tooling handles operations such as:

- duplicate removal
- missing-value handling
- string normalization
- whitespace cleanup
- restructuring source columns
- preparing data for SQL import

---

## Architecture

The application follows a server-rendered **MVC architecture**.

```text
Browser
   │
   ▼
PHP Controllers
   │
   ├──── Views
   │
   ▼
Model
   │
   ▼
MySQL
```

### Controllers

Application flows are separated into dedicated controllers, including:

```text
Controller_administration.php
Controller_connexion_inscription.php
Controller_exportation.php
Controller_historique.php
Controller_home.php
Controller_list.php
Controller_monCompte.php
Controller_recherche.php
Controller_set.php
```

They coordinate user actions, validation, sessions and interactions with the data layer.

### Model

The application uses a central PHP model for database access and business operations.

It handles data related to:

- games
- physical boxes
- users
- borrowers
- loans
- locations
- collections
- authors
- publishers
- categories
- mechanisms
- history

### Relational database

The MySQL schema separates the collection into normalized entities.

Some of the principal tables are:

```text
jeu
boite
utilisateur
emprunteur
pret
historique
localisation
collection
auteur
editeur
categorie
mecanisme
```

Many-to-many relationships are represented through association tables:

```text
jeu_auteur
jeu_editeur
jeu_categorie
jeu_mecanisme
```

---

## My Contribution

This was a collaborative project.

My contribution focused primarily on:

- PHP authentication and registration flows
- user account management
- form validation and account-related interactions
- Python data-cleaning and normalization tooling
- preparation of the large source dataset for relational storage
- contributions to SQL import and testing workflows
- integration work across the application and database

The project gave me practical experience working on an existing shared codebase rather than building an isolated individual application.

---

## Tech Stack

| Area | Technologies |
| --- | --- |
| Backend | PHP |
| Database | MySQL, SQL |
| Data processing | Python, Pandas, OpenPyXL |
| Excel export | Python, OpenPyXL, MySQL Connector |
| Frontend | HTML, CSS, JavaScript |
| Architecture | MVC, relational data modelling |
| Environment | Apache / XAMPP |

---

## Project Structure

```text
university-board-game-library/
│
├── app/
│   ├── Controllers/
│   ├── Models/
│   ├── Views/
│   ├── Content/
│   ├── identifiants/
│   ├── scripts/
│   │   ├── data/
│   │   └── scripts_exportation/
│   └── index.php
│
├── SQL/
│   ├── creation_tables.sql
│   ├── script_insertion.sql
│   └── Model EA modifié.pdf
│
├── scripts/
│   ├── data/
│   └── scripts_nettoyage/
│       ├── nettoyage.py
│       ├── utils.py
│       └── utils_test.py
│
├── archive/
└── README.md
```

---

## Getting Started

### Requirements

You will need:

- PHP
- Apache or XAMPP
- MySQL
- Python 3
- pip
- a modern web browser

Python dependencies used by the project include:

```text
pandas
openpyxl
mysql-connector-python
```

---

### 1. Clone the repository

```bash
git clone https://github.com/Lasryy/university-board-game-library.git
cd university-board-game-library
```

---

### 2. Prepare the database

The SQL schema is located in:

```text
SQL/creation_tables.sql
```

The current script imports the original inventory through `LOAD DATA INFILE`.

Before running it, update the inventory path according to your MySQL installation.

For example:

```sql
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 9.1/Uploads/inventaire.csv'
INTO TABLE TempJeux
...
```

Then run:

```text
SQL/creation_tables.sql
SQL/script_insertion.sql
```

in that order.

> The repository was developed primarily in a Windows/XAMPP environment, so local paths may need to be adapted for another setup.

---

### 3. Configure the database connection

Configure your local database credentials in:

```text
app/identifiants/identifiant.php
```

The application expects values such as:

```php
$dsn = 'mysql:host=localhost;dbname=your_database';
$host = 'localhost';
$username = 'your_username';
$password = 'your_password';
$database = 'your_database';
```

Do not commit real credentials to a public repository.

---

### 4. Install Python dependencies

```bash
pip install pandas openpyxl mysql-connector-python
```

The source-data cleaning utilities are located in:

```text
scripts/scripts_nettoyage/
```

The Excel export tooling is located in:

```text
app/scripts/scripts_exportation/
```

---

### 5. Run the application

With XAMPP, place the application inside the Apache web root.

For example:

```text
C:/xampp/htdocs/app/
```

Start:

```text
Apache
MySQL
```

Then open:

```text
http://localhost/app/
```

---

## Team

Developed collaboratively by:

- [Lasry BESKIWIN](https://github.com/Lasryy)
- [Rania BOUSFIHA](https://github.com/rania212)
- [Safiya NGUYEN](https://github.com/safiya-ng)
- [Ahash PARTHIPAN](https://github.com/AhashPARTHIPAN)
- [Jules RICHARDOT](https://github.com/JulesRichardot)

---

## Academic Context

This application was developed at **Université Sorbonne Paris Nord** as part of a university software-development project.

The project involved several complementary software-engineering challenges:

- processing a large heterogeneous dataset
- migrating spreadsheet data into a relational model
- designing SQL relationships
- integrating PHP and MySQL
- structuring a web application using MVC
- implementing authentication and role-based workflows
- managing loans and historical information
- building administrative interfaces
- collaborating through a shared codebase

---

## What This Project Demonstrates

This project demonstrates practical experience with:

- full-stack web development
- PHP MVC architecture
- relational database modelling
- SQL and MySQL
- authentication and session management
- large dataset processing
- Python automation
- Pandas and OpenPyXL
- data migration
- collaborative development
- business-oriented application design

---

<details>
<summary><strong>🇫🇷 Version française</strong></summary>

<br>

# Ludothèque universitaire

Application web full stack conçue pour organiser, rechercher et gérer une collection universitaire de plus de **17 000 jeux de société**, dont certains remontent au XIXe siècle.

Développé en équipe à **l'Université Sorbonne Paris Nord**, le projet combine une application **PHP en architecture MVC**, une base relationnelle **MySQL** et des outils de traitement de données en **Python**.

[Voir l'étude de cas complète sur mon portfolio](https://lasrybeskiwin.fr/work/university-board-game-library/)

![Aperçu de la ludothèque universitaire](archive/App/assets/images/site-demo2.gif)

---

## Présentation

La collection était initialement décrite dans plusieurs fichiers de données nécessitant un important travail de nettoyage et de restructuration.

Le projet a donc consisté à construire à la fois :

- un pipeline de préparation des données
- une base de données relationnelle
- une application web métier permettant d'exploiter la collection

L'application permet notamment :

- la consultation de plus de **17 000 jeux**
- la recherche multicritère
- la gestion des exemplaires physiques
- la localisation des boîtes
- la gestion des prêts
- le suivi de l'historique
- la gestion de plusieurs rôles utilisateurs
- l'administration de la collection
- l'export des données vers Excel

---

## Fonctionnalités principales

### Collection

- Recherche dans le catalogue
- Filtres multicritères
- Informations détaillées sur les jeux
- Gestion des exemplaires
- Localisation physique
- État des boîtes
- Codes-barres
- Auteurs, éditeurs, catégories et mécanismes

### Prêts

- Enregistrement des prêts
- Suivi des retours
- Gestion de la disponibilité
- Historique
- Administration des emprunts

### Comptes utilisateurs

L'application dispose d'un système de comptes comprenant :

- inscription
- connexion
- validation des formulaires
- sessions PHP
- hachage des mots de passe avec **Argon2id**
- modification des informations du profil
- changement de mot de passe

Le modèle distingue trois rôles :

```text
Admin
Gestionnaire
Utilisateur
```

### Export

Un administrateur peut déclencher depuis l'application un script Python qui interroge MySQL et génère un fichier Excel contenant les informations de la collection.

---

## Pipeline de données

```text
Données Excel
      │
      ▼
Python
Pandas / OpenPyXL
      │
      ▼
Nettoyage & normalisation
      │
      ▼
Données structurées
      │
      ▼
Import MySQL
      │
      ▼
Base relationnelle
      │
      ▼
Application PHP MVC
```

---

## Architecture

```text
Navigateur
    │
    ▼
Contrôleurs PHP
    │
    ├──── Vues
    │
    ▼
Modèle
    │
    ▼
MySQL
```

La base sépare notamment :

```text
jeu
boite
utilisateur
emprunteur
pret
historique
localisation
collection
auteur
editeur
categorie
mecanisme
```

avec des tables de liaison telles que :

```text
jeu_auteur
jeu_editeur
jeu_categorie
jeu_mecanisme
```

---

## Ma contribution

Ce projet a été réalisé collectivement.

Ma contribution s'est principalement concentrée sur :

- les parcours PHP d'inscription et d'authentification
- la gestion du compte utilisateur
- la validation des formulaires liés aux comptes
- les scripts Python de nettoyage et de normalisation
- la préparation des données sources pour leur stockage relationnel
- des contributions aux scripts d'import et de test SQL
- l'intégration entre plusieurs parties de l'application

---

## Stack technique

| Domaine | Technologies |
| --- | --- |
| Backend | PHP |
| Base de données | MySQL, SQL |
| Traitement de données | Python, Pandas, OpenPyXL |
| Export Excel | Python, OpenPyXL, MySQL Connector |
| Frontend | HTML, CSS, JavaScript |
| Architecture | MVC, modèle relationnel |
| Environnement | Apache / XAMPP |

---

## Installation

### Cloner le projet

```bash
git clone https://github.com/Lasryy/university-board-game-library.git
cd university-board-game-library
```

### Préparer MySQL

Adapter le chemin de `inventaire.csv` dans :

```text
SQL/creation_tables.sql
```

puis exécuter :

```text
SQL/creation_tables.sql
SQL/script_insertion.sql
```

### Configurer la connexion

Modifier :

```text
app/identifiants/identifiant.php
```

avec les identifiants de votre environnement local.

### Installer les dépendances Python

```bash
pip install pandas openpyxl mysql-connector-python
```

### Lancer l'application

Placer `app/` dans le répertoire web Apache/XAMPP, démarrer Apache et MySQL puis ouvrir :

```text
http://localhost/app/
```

---

## Équipe

Projet développé avec :

- [Lasry BESKIWIN](https://github.com/Lasryy)
- [Rania BOUSFIHA](https://github.com/rania212)
- [Safiya NGUYEN](https://github.com/safiya-ng)
- [Ahash PARTHIPAN](https://github.com/AhashPARTHIPAN)
- [Jules RICHARDOT](https://github.com/JulesRichardot)

---

## Ce que ce projet démontre

Ce projet met notamment en avant :

- le développement web full stack
- l'architecture MVC en PHP
- la modélisation relationnelle
- SQL et MySQL
- l'authentification et la gestion de sessions
- le traitement de données volumineuses
- l'automatisation en Python
- Pandas et OpenPyXL
- la migration de données
- le développement collaboratif
- la conception d'une application répondant à des besoins métier

</details>
