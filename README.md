# Travaux Pratiques DevOps : Git & GitHub

**Formation :** Master 2 - Big Data  
**Auteur :** Molka Jebali (molka.jebali.m2025@ihec.ucar.tn)  
**Module :** DevOps & Intégration Continue  
**Date :** Novembre 2026  

---

## 📂 Structure du Répertoire de Rendu

Ce dépôt regroupe les réalisations et les comptes rendus détaillés des 3 Travaux Pratiques, disponibles en consultation directe Markdown (`.md`) et en téléchargement Word (`.docx`) :

```text
TPs/
├── 📄 README.md                    <-- Présentation générale et index des livrables
│
├── 📑 Rendu_TP1_Git.md             <-- Compte rendu en ligne TP1 (Dépôt local)
├── 📑 Rendu_TP2_GitHub.md          <-- Compte rendu en ligne TP2 (Dépôt distant & Sécurité)
├── 📑 Rendu_TP3_Branche.md         <-- Compte rendu en ligne TP3 (Gestion des branches)
│
├── 📘 Rendu TP1.docx               <-- Document Word TP1 (avec captures d'écran)
├── 📘 Rendu TP2.docx               <-- Document Word TP2 (avec captures d'écran)
├── 📘 Rendu TP3.docx               <-- Document Word TP3 (avec captures d'écran)
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

### 1. TP1 : Git — Création d’un dépôt local
* **Objectifs :** Configuration de l'environnement (`user.name`, `user.email`), initialisation de dépôts (`git init`), compréhension des 3 zones Git (Working Directory, Staging Area, Local Repository), gestion des commits et analyse de l'historique (`git log`).
* 📄 **Lecture en ligne (Markdown) :** [Consulter Rendu_TP1_Git.md](./Rendu_TP1_Git.md)
* 📥 **Version Word :** [Télécharger Rendu TP1.docx](./Rendu%20TP1.docx)

### 2. TP2 : GitHub — Dépôt distant, Sécurité et Bonnes Pratiques
* **Objectifs :** Synchronisation bidirectionnelle (`clone`, `push`, `pull`), rôle et importance critique du fichier `.gitignore`, modèles de sécurité modernes (dépréciation des mots de passe en 2021, utilisation des **Tokens d'Accès Personnels PAT** et des **paires de clés asymétriques SSH ed25519**).
* 📄 **Lecture en ligne (Markdown) :** [Consulter Rendu_TP2_GitHub.md](./Rendu_TP2_GitHub.md)
* 📥 **Version Word :** [Télécharger Rendu TP2.docx](./Rendu%20TP2.docx)

### 3. TP3 : Git — Gestion des Branches et Stratégies de Versioning
* **Objectifs :** Isolation du développement par branches (`git branch`, `git checkout` / `switch`), manipulation du pointeur `HEAD`, inspection fine des modifications avec `git diff`, résolution de bugs en équipe et procédures de rollback / restauration (`git checkout -- .`, `git reset`, `git stash`).
* 📄 **Lecture en ligne (Markdown) :** [Consulter Rendu_TP3_Branche.md](./Rendu_TP3_Branche.md)
* 📥 **Version Word :** [Télécharger Rendu TP3.docx](./Rendu%20TP3.docx)

---

## 🛠️ Outils & Environnement Utilisés
* **Système d'exploitation :** Windows 11
* **Outil de versioning :** Git 2.51.2
* **Plateforme distante :** GitHub
* **Terminal :** PowerShell / Git Bash
