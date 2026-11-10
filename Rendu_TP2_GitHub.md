# Compte Rendu - TP2 : GitHub — Dépôt distant, Sécurité et Bonnes Pratiques

**Module :** DevOps / Gestion de Versions Distribuées  
**Niveau :** Master 2 - Big Data  
**Sujet :** TP2 : GitHub -- Création d’un dépôt distant avec Git et GitHub : Fondamentaux, Sécurité et Bonnes Pratiques  
**Date :** 10 Novembre 2026  

---

## 1. Objectifs du TP
- Maîtriser l'intégration entre Git (local) et GitHub (distant / hébergé dans le cloud).
- Comprendre le modèle distribué (DVCS) et les flux de synchronisation bidirectionnels (`push`, `pull`, `clone`).
- Maîtriser les mécanismes modernes et stricts d'authentification sécurisée :
  - **Tokens d'accès personnels (PAT - Personal Access Tokens)** via HTTPS.
  - **Paires de clés asymétriques (SSH ed25519 / RSA)**.
- Comprendre les politiques de droits d'accès (dépôts publics vs privés, lecture anonyme vs écriture authentifiée).

---

## 2. Travail Demandé & Réponses Détaillées aux Questions

### A. Préparation initiale

#### 1) Création d'un compte sur le site GitHub
- Inscription sur [GitHub](https://github.com/) avec une adresse institutionnelle ou personnelle.
- Validation de l'adresse email et sécurisation du compte (activation recommandée de la double authentification - 2FA).
- Accès au tableau de bord personnel.

---

### B. Création du repository distant

#### 2) Création du repository « First_Project »
- Dans GitHub, cliquer sur le bouton **« New »** (ou icône `+` en haut à droite -> *New repository*).
- **Repository name :** `First_Project`
- **Visibilité :** Sélectionner **Public** (le code est lisible par tout le monde sur Internet).
- Options d'initialisation : cocher ou non la création d'un README (ou laisser vide pour démarrer).

#### 3) À quoi sert le fichier « .gitignore » ? (Question théorique fondamentale)
Le fichier `.gitignore` est un fichier de configuration textuel placé à la racine (ou dans les sous-dossiers) d'un dépôt Git. Il contient des règles et des motifs (wildcards) indiquant à Git quels fichiers, dossiers ou extensions **ne doivent jamais être suivis (untracked)** ni intégrés dans l'historique des versions.

**Pourquoi est-il indispensable ?**
1. **Sécurité critique :** Empêcher le commit accidentel de secrets sensibles (fichiers `.env`, clés d'API privées, certificats SSL, mots de passe de bases de données, tokens OAuth).
2. **Performance et taille du dépôt :** Éviter de versionner des artefacts de compilation lourds, des dépendances tierces régénérables (`node_modules/`, `target/`, `.venv/`, fichiers `.class`, `.exe`, `.dll`).
3. **Propreté et absence de conflits :** Ignorer les fichiers temporaires et les caches spécifiques aux IDEs et systèmes d'exploitation (`.vscode/`, `.idea/`, `.DS_Store`, `Thumbs.db`, fichiers temporaires `*.tmp`).

**Exemple de contenu type pour un projet DevOps / Python :**
```gitignore
# Dépendances et environnements virtuels
venv/
__pycache__/
*.pyc

# Variables d'environnement et secrets
.env
*.pem
credentials.json

# Caches et logs
*.log
.vscode/
.idea/
```

#### 4) Création du fichier « FirstFile » sur le repository distant
- Sur l'interface web GitHub du repository `First_Project` :
  1. Cliquer sur **« Add file »** -> **« Create new file »**.
  2. Nommer le fichier : `FirstFile`.
  3. Contenu saisi : `Bienvenue dans First_Project - Cree sur le repository distant GitHub.`
  4. Saisir un message de commit (ex: *"Initial commit on GitHub: Creation of FirstFile"*).
  5. Cliquer sur le bouton vert **« Commit changes... »**.

---

### C. Clonage du projet sur la machine locale

#### 5) Commande de clonage via HTTPS
- Récupérer l'URL HTTPS sur GitHub via le bouton vert **« Code »** -> onglet **HTTPS** :
  `https://github.com/<Nom-Utilisateur>/First_Project.git`
- Lancer le clonage depuis le terminal :
```bash
git clone https://github.com/<Nom-Utilisateur>/First_Project.git
```

*Fonctionnement interne :* Cette commande télécharge l'intégralité de l'historique du dépôt distant, initialise un dépôt Git local, extrait la branche par défaut (`main`) dans le répertoire de travail, et configure automatiquement un remote nommé `origin` pointant vers cette URL.

#### 6) Vérification du succès du clonage
```text
Cloning into 'First_Project'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

#### 7) Positionnement sous le répertoire cloné
```bash
cd First_Project
```

#### 8) Consultation de la liste des fichiers existants
```bash
dir
# ou : ls -la
```
**Résultat :**
```text
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/10/2026 12:55 PM             71 FirstFile
```
Le fichier `FirstFile` créé sur GitHub est bien présent localement.

---

### D. Gestion, suivi et vérification des fichiers

#### 9) Création du fichier local « LocalFile »
```bash
echo "Contenu de LocalFile redige en local." > LocalFile
```

#### 10) Ajout à l'index et commit local
```bash
git add LocalFile
git commit -m "Ajout du fichier LocalFile en local"
```

**Résultat du terminal :**
```text
[main d19cac3] Ajout du fichier LocalFile en local
 1 file changed, 1 insertion(+)
 create mode 100644 LocalFile
```

#### 11) Vérification de l'état et consultation du log
```bash
git status
```
**Résultat obtenu :**
```text
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

```bash
git log --oneline
```
**Résultat obtenu :**
```text
d19cac3 (HEAD -> main) Ajout du fichier LocalFile en local
b4ea2ab (origin/main, origin/HEAD) Initial commit on GitHub: Creation of FirstFile
```

#### 12) Vérification sur le repository distant : Le fichier `LocalFile` existe-t-il sur GitHub ? Pourquoi ?
- **Réponse : NON**, le fichier `LocalFile` n'existe pas encore sur le repository distant GitHub.
- **Pourquoi ? (Principe d'architecture DVCS) :**  
  Git est un système de gestion de versions **totalement distribué**. Les commandes `git add` et `git commit` s'exécutent **strictement en local** sur la machine du développeur, modifiant uniquement la base de données interne locale située dans `.git`.  
  Tant qu'aucune opération réseau explicite de synchronisation (`git push`) n'a été déclenchée, le serveur distant (GitHub) ignore totalement les modifications apportées. Comme le montre le message :
  `Your branch is ahead of 'origin/main' by 1 commit.` : la branche locale a 1 commit d'avance sur la branche distante.

---

### E. Synchronisation avec le Repository distant

#### 13) Exécution de la commande `git push`
```bash
git push origin main
```

#### 14) Que se passe-t-il ? Pourquoi ?
- **Que se passe-t-il ?**  
  Git ouvre une invite d'authentification ou échoue avec une erreur d'autorisation si l'utilisateur tente de saisir son mot de passe de compte classique GitHub :
  ```text
  remote: Support for password authentication was removed on August 13, 2021.
  remote: Please see https://gh.io/gcpp for more information.
  fatal: Authentication failed for 'https://github.com/.../First_Project.git/'
  ```
- **Pourquoi ?**  
  Depuis le **13 août 2021**, GitHub a définitivement révoqué l'authentification par simple mot de passe de compte pour toutes les opérations Git via HTTPS pour des raisons de sécurité critiques (vulnérabilité aux attaques par force brute, vol d'identifiants, absence de limitation de périmètre).  
  Désormais, GitHub impose l'usage :
  1. Soit d'un **Personal Access Token (PAT)** avec privilèges restreints et date d'expiration.
  2. Soit de clés cryptographiques **SSH**.

---

### Authentification par tokens (PAT - Personal Access Token)

#### 15) Procédure de génération d'un Personal Access Token sur GitHub
1. Sur GitHub, cliquer sur l'avatar du profil (en haut à droite) -> **Settings**.
2. Dans le menu de gauche, faire défiler vers le bas et cliquer sur **Developer settings**.
3. Sélectionner **Personal access tokens** -> **Tokens (classic)** (ou *Fine-grained tokens*).
4. Cliquer sur **Generate new token** -> **Generate new token (classic)**.
5. Renseigner le formulaire :
   - **Note :** `Token-DevOps-TP2`
   - **Expiration :** Définir une durée de validité (ex: 30 jours, 90 jours).
   - **Select scopes (Privilèges) :** Cocher la case principale **`repo`** (donne l'accès complet aux dépôts publics et privés : commit, push, pull, gestion des branches).
6. Cliquer sur le bouton vert **« Generate token »** en bas de la page.

#### 16) Sauvegarde du token
- GitHub affiche le jeton généré (chaîne commençant généralement par `ghp_...`).
> **Important :** Ce jeton n'est affiché qu'une **seule et unique fois**. Il faut le copier immédiatement et le conserver en sécurité (dans un gestionnaire de mots de passe). Si vous quittez la page sans le copier, il devra être régénéré.

#### 17) Exécution du `git push` avec le token
Lors de l'invite de commande de Git :
- **Username :** Votre identifiant GitHub.
- **Password :** Coller le token généré (`ghp_...`) à la place du mot de passe.

Alternative via l'URL avec token :
```bash
git push https://<TOKEN>@github.com/<Nom-Utilisateur>/First_Project.git main
```

#### 18) Consultation de la mise à jour distante
En rafraîchissant l'interface web de GitHub sur `First_Project`, le fichier `LocalFile` et son commit apparaissent désormais dans la liste des fichiers du dépôt en ligne.

#### 19) Synchronisation inverse avec `git pull`
- Sur l'interface web GitHub, créer un nouveau fichier nommé `RemoteFile2` et valider le commit.
- Pour récupérer ces modifications en local, exécuter dans le terminal :
```bash
git pull origin main
```
**Résultat obtenu :**
```text
From https://github.com/.../First_Project
 * branch            main       -> FETCH_HEAD
   d19cac3..44659f6  main       -> origin/main
Updating d19cac3..44659f6
Fast-forward
 RemoteFile2 | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 RemoteFile2
```
Le fichier `RemoteFile2` est désormais présent sur le disque dur local.

#### 20) Pourquoi `git pull` n’a pas demandé une authentification, contrairement à l’action de `git push` ?
- **Explication fondamentale sur les autorisations :**  
  Le dépôt `First_Project` a été configuré comme **Public** (Étape B.2).
  - **Action de Lecture (`git pull`, `git clone`, `git fetch`) :** Sur un dépôt public, le code source est ouvert en consultation libre. N'importe qui sur Internet a le droit de lire et de télécharger le projet sans avoir besoin d'être authentifié ni identifié.
  - **Action d'Écriture (`git push`) :** Pousser du code modifie l'historique officiel du dépôt distant. Pour des raisons évidentes d'intégrité et de sécurité, GitHub doit vérifier formellement que la personne qui pousse possède les droits de modification (propriétaire ou collaborateur avec droit *write*). L'authentification (Token ou SSH) est donc strictement obligatoire pour le `push`.

---

### Authentification par SSH (Secure Shell Protocol)

L'authentification SSH repose sur le principe de la cryptographie asymétrique avec une **paire de clés** :
- **Clé privée :** Reste strictement sur la machine locale du développeur, secrète et jamais transmise.
- **Clé publique :** Déposée sur GitHub pour vérifier mathématiquement la signature de la clé privée lors des échanges.

#### 21) Génération d'une clé SSH
Dans le terminal (PowerShell ou Git Bash) :
```bash
ssh-keygen -t ed25519 -C "molka.jebali.m2025@ihec.ucar.tn"
```
*(Appuyer sur Entrée pour accepter l'emplacement par défaut `~/.ssh/id_ed25519` et définir éventuellement une passphrase).*

Affichage de la clé publique générée pour la copier :
```bash
cat ~/.ssh/id_ed25519.pub
```
*Exemple de clé publique obtenue :*
`ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIG1kF0wz... molka.jebali.m2025@ihec.ucar.tn`

#### 22) Ajout de la clé publique sur GitHub
1. Sur GitHub, aller dans **Settings** -> **SSH and GPG keys**.
2. Cliquer sur le bouton vert **« New SSH key »**.
3. Renseigner le formulaire :
   - **Title :** `Machine-DevOps-Portable`
   - **Key type :** `Authentication Key`
   - **Key :** Coller l'intégralité du contenu de `id_ed25519.pub`.
4. Valider en cliquant sur **« Add SSH key »**.

#### 23) Vérification de la connexion SSH
```bash
ssh -T git@github.com
```
**Résultat attendu :**
```text
The authenticity of host 'github.com (140.82.121.4)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Hi MolkaJebali! You've successfully authenticated, but GitHub does not provide shell access.
```
Ce message confirme que GitHub a identifié l'utilisateur avec succès grâce à sa clé SSH.

#### 24) Suppression du projet local existant
```bash
cd ..
rm -rf First_Project
# Ou sous PowerShell : Remove-Item -Recurse -Force First_Project
```

#### 25) Clonage du projet via l'URL SSH
- Sur GitHub -> bouton vert **« Code »** -> onglet **SSH** :
  `git@github.com:<Nom-Utilisateur>/First_Project.git`
- Lancer le clonage :
```bash
git clone git@github.com:<Nom-Utilisateur>/First_Project.git
```

#### 26) Vérification du clonage réussi
```bash
cd First_Project
git remote -v
```
**Résultat :**
```text
origin  git@github.com:MolkaJebali/First_Project.git (fetch)
origin  git@github.com:MolkaJebali/First_Project.git (push)
```
Désormais, tous les `git push` et `git pull` se dérouleront de manière fluide, chiffrée et transparente, sans jamais demander de mot de passe ni de token dans le terminal.

---

## 3. Synthèse comparative : Token HTTPS vs Clé SSH

| Critère | Token d'Accès Personnel (PAT HTTPS) | Clé SSH (Asymétrique) |
|---|---|---|
| **Protocole** | HTTPS (Port 443) | SSH (Port 22) |
| **Facilité réseau** | Fonctionne derrière tous les proxies/firewalls d'entreprise | Peut nécessiter une ouverture du port 22 |
| **Confort au quotidien** | Nécessite un gestionnaire de credentials ou ressaisie | Connexion automatique sans mot de passe |
| **Expiration** | Expiration obligatoire ou fortement recommandée | Clé persistante jusqu'à révocation explicite |
| **Sécurité** | Scopes granulaires (lecture seule, repo, etc.) | Sécurisable par une passphrase locale forte |

---
**Conclusion :** Le TP2 est validé. La gestion des dépôts distants sur GitHub, les transferts bidirectionnels (`push`/`pull`) et les deux modes d'authentification sécurisée (PAT et SSH) sont pleinement maîtrisés.

