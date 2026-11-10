# Travaux Pratiques DevOps : Git & GitHub

**Formation :** Master 2 - Big Data  
**Auteur :** Molka Jebali (molka.jebali.m2025@ihec.ucar.tn)  
**Module :** DevOps & Intégration Continue  
**Date :** Novembre 2026  

---

## 📂 Structure du Répertoire de Rendu

Ce dépôt regroupe les réalisations et les comptes rendus détaillés des 3 Travaux Pratiques :

```text
TPs/
├── 📄 README.md                    <-- Présentation générale et index des livrables
│
├── 📑 Rendu_TP1_Git.md             <-- Compte rendu complet du TP1 (Dépôt local)
├── 📑 Rendu_TP2_GitHub.md          <-- Compte rendu complet du TP2 (Dépôt distant & Sécurité)
├── 📑 Rendu_TP3_Branche.md         <-- Compte rendu complet du TP3 (Gestion des branches)
│
├── 📂 TP1/                         <-- Répertoire du TP1
│   ├── file1.txt, file2.txt, file3.txt, file4.txt
│   └── Rendu_TP1_Git.md
│
├── 📂 TP2/                         <-- Répertoire du TP2
│   ├── First_Project/ (FirstFile, LocalFile, RemoteFile2)
│   └── Rendu_TP2_GitHub.md
│
└── 📂 TP3/                         <-- Répertoire du TP3
    ├── base.txt, filebranch1.txt, file_extra.txt, test_rollback.txt
    └── Rendu_TP3_Branche.md
```

---

## 📌 Sommaire des Travaux Pratiques

### 1. [TP1 : Git — Création d’un dépôt local](Rendu_TP1_Git.md)
* **Objectifs :** Configuration de l'environnement (`user.name`, `user.email`), initialisation de dépôts (`git init`), compréhension des 3 zones Git (Working Directory, Staging Area, Local Repository), gestion des commits et analyse de l'historique (`git log`).
* **Livrable détaillé :** [Consulter le Rendu TP1](Rendu_TP1_Git.md)

### 2. [TP2 : GitHub — Dépôt distant, Sécurité et Bonnes Pratiques](Rendu_TP2_GitHub.md)
* **Objectifs :** Synchronisation bidirectionnelle (`clone`, `push`, `pull`), rôle et importance critique du fichier `.gitignore`, modèles de sécurité modernes (dépréciation des mots de passe en 2021, utilisation des **Tokens d'Accès Personnels PAT** et des **paires de clés asymétriques SSH ed25519**).
* **Livrable détaillé :** [Consulter le Rendu TP2](Rendu_TP2_GitHub.md)

### 3. [TP3 : Git — Gestion des Branches et Stratégies de Versioning](Rendu_TP3_Branche.md)
* **Objectifs :** Isolation du développement par branches (`git branch`, `git checkout` / `switch`), manipulation du pointeur `HEAD`, inspection fine des modifications avec `git diff`, résolution de bugs en équipe et procédures de rollback / restauration (`git checkout -- .`, `git reset`, `git stash`).
* **Livrable détaillé :** [Consulter le Rendu TP3](Rendu_TP3_Branche.md)

---

## 🛠️ Outils & Environnement Utilisés
* **Système d'exploitation :** Windows 11
* **Outil de versioning :** Git 2.51.2
* **Plateforme distante :** GitHub
* **Terminal :** PowerShell / Git Bash

