# Compte Rendu - TP1 : Git — Création d'un dépôt local

**Module :** DevOps / Gestion de Versions  
**Niveau :** Master 2 - Big Data  
**Sujet :** TP1 : GiT -- Création d’un dépôt local  
**Date :** 10 Novembre 2026  

---

## 1. Objectifs du TP
- Acquérir une compréhension approfondie des principes fondamentaux et de l'architecture interne de Git.
- Configurer et gérer un environnement de travail Git, incluant l'identification des auteurs (`user.name`, `user.email`).
- Pratiquer la création de dépôts locaux (`git init`), le cycle de vie des fichiers (`untracked`, `staged`, `committed`).
- Maîtriser l'historique des modifications, la navigation dans les commits (`git log`) et la gestion des configurations multi-niveaux.

---

## 2. Rappel conceptuel : L'architecture des 3 zones de Git

Git organise le suivi des fichiers à travers trois zones principales :

```
+------------------------+      git add       +------------------------+     git commit     +------------------------+
|   Working Directory    |  =============>    |      Staging Area      |   =============>   |   Local Repository     |
| (Espace de Travail)    |                    |     (Index / Cache)    |                    |        (.git)          |
| Fichiers modifiés /    |  <=============    | Fichiers prêts pour    |   <=============   | Historique permanent   |
| non suivis (Untracked) |    git checkout    | le prochain commit     |     git reset      | des snapshots validés  |
+------------------------+                    +------------------------+                    +------------------------+
```

1. **Working Directory (Répertoire de travail) :** Le dossier physique sur le disque où les fichiers sont créés, modifiés et consultés.
2. **Staging Area (Index) :** Zone tampon intermédiaire préparant le prochain instantané (snapshot).
3. **Repository (Dépôt Git / `.git`) :** Base de données contenant l'historique complet et immuable des commits (objets blobs, trees, commits).

---

## 3. Travail Demandé & Réponses Détaillées

### A. Configuration des informations utilisateur

#### 1) Configuration du `user.name` et `user.email`
Git associe chaque commit à une identité d'auteur. La configuration globale s'applique à tous les dépôts de l'utilisateur sur la machine, tandis que la configuration locale s'applique spécifiquement au dépôt courant.

**Commandes exécutées :**
```bash
git config --global user.name "Molka Jebali"
git config --global user.email "molka.jebali.m2025@ihec.ucar.tn"
```

Pour vérifier les paramètres appliqués :
```bash
git config user.name
# Sortie : Molka Jebali

git config user.email
# Sortie : molka.jebali.m2025@ihec.ucar.tn
```

*Explication :* Ces informations sont inscrites dans le fichier de configuration utilisateur `~/.gitconfig` (sur Windows : `C:\Users\<Utilisateur>\.gitconfig`).

---

### B. Initialisation du projet

#### 2) Création du dossier « TP1 » et positionnement
```bash
mkdir TP1
cd TP1
```

#### 3) Initialisation du dépôt Git
```bash
git init
```

**Résultat obtenu :**
```text
Initialized empty Git repository in C:/Users/Lenovo/Desktop/M2-BigData/Devops/TPs/TP1/.git/
```

#### 4) Liste des éléments existants (vérification du dossier masqué `.git`)
Sous Linux / Bash :
```bash
ls -la
```
Sous PowerShell :
```powershell
Get-ChildItem -Force
```

**Résultat obtenu :**
```text
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d--h-          11/10/2026 12:53 PM                .git
```

*Analyse :* Le répertoire caché `.git` constitue le cœur du dépôt. Il contient :
- `HEAD` : référence de la branche courante.
- `config` : configuration propre au dépôt local.
- `objects/` : stockage compressé de tous les blobs (fichiers), trees (dossiers) et commits.
- `refs/` : pointeurs vers les branches (`heads`) et les tags.

---

### C. Création et suivi des fichiers

#### 5) Création du fichier `file1.txt`
```bash
echo "Ceci est le contenu de file1.txt pour le TP1 Git." > file1.txt
```

#### 6) Affichage du contenu
```bash
cat file1.txt
# Ou sous PowerShell : Get-Content file1.txt
```
**Contenu affiché :**
```text
Ceci est le contenu de file1.txt pour le TP1 Git.
Initialisation du projet et suivi de version avec Git.
```

#### 7) Vérification de l'état avec `git status`
```bash
git status
```

**Résultat obtenu dans le terminal :**
```text
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	file1.txt

nothing added to commit but untracked files present (use "git add" to track)
```

**Analyse des 3 zones identifiées :**
- **Branch Master (ou main) :** Indique la branche active sur laquelle le travail s'effectue (`On branch master`).
- **Untracked Area (Fichiers non suivis) :** `file1.txt` est présent dans le répertoire de travail physique, mais Git ne l'a jamais indexé. Il n'appartient pas encore à l'historique du projet.
- **Staging Area :** Pour le moment vide (`nothing added to commit`).

#### 8) Indexation avec `git add file1.txt`
```bash
git add file1.txt
git status
```

**Résultat obtenu :**
```text
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   file1.txt
```

**Remarques importantes :**
- Le fichier `file1.txt` est passé de la **Untracked Area** vers la **Staging Area** (état `staged` / "Changes to be committed"). Git a calculé le hash SHA-1 de son contenu et l'a placé en attente de commit.
- Pour indexer l'ensemble des fichiers modifiés et nouveaux sous le répertoire courant en une seule commande, on utilise :
  ```bash
  git add .
  ```
  ou :
  ```bash
  git add -A
  ```

---

### D. Gestion des commits

#### 9) Premier commit
```bash
git commit -m "Premier commit : ajout de file1.txt"
```

**Résultat obtenu :**
```text
[master (root-commit) bab88c3] Premier commit : ajout de file1.txt
 1 file changed, 2 insertions(+)
 create mode 100644 file1.txt
```

*Explication :* L'indicateur `(root-commit)` signale qu'il s'agit du tout premier commit de l'historique du dépôt. Un identifiant unique (hash SHA-1 condensé : `bab88c3`) lui a été attribué.

#### 10) Création de `file2.txt` et `file3.txt`, indexation et deuxième commit
```bash
echo "Deuxieme fichier pour le TP1." > file2.txt
echo "Troisieme fichier pour le TP1." > file3.txt
git add .
git commit -m "Deuxieme commit : ajout de file2.txt et file3.txt"
```

**Résultat obtenu :**
```text
[master 545b620] Deuxieme commit : ajout de file2.txt et file3.txt
 2 files changed, 2 insertions(+)
 create mode 100644 file2.txt
 create mode 100644 file3.txt
```

#### 11) Consultation de l'historique avec `git log`
```bash
git log
```

**Résultat complet obtenu :**
```text
commit 545b62074a64b443f082334e486124b1edcc9b6e (HEAD -> master)
Author: Molka Jebali <molka.jebali.m2025@ihec.ucar.tn>
Date:   Tue Nov 10 12:53:45 2026 +0100

    Deuxieme commit : ajout de file2.txt et file3.txt

commit bab88c3289f40ce3ce47cc6ecb59ee56a55ba095
Author: Molka Jebali <molka.jebali.m2025@ihec.ucar.tn>
Date:   Tue Nov 10 12:53:44 2026 +0100

    Premier commit : ajout de file1.txt
```

#### 12) Limitation aux 2 derniers commits avec `git log -nbre`
```bash
git log -2
```
*(Affiche exactement les 2 derniers commits ci-dessus).*

#### 13) Affichage condensé sur une ligne avec `git log --oneline`
```bash
git log --oneline
```

**Résultat obtenu :**
```text
545b620 (HEAD -> master) Deuxieme commit : ajout de file2.txt et file3.txt
bab88c3 Premier commit : ajout de file1.txt
```

*Explication :* `git log --oneline` affiche chaque commit sur une ligne unique en tronquant le hash SHA-1 aux 7 premiers caractères et en affichant uniquement le message du commit.

---

### E. Vérification de la configuration de l'utilisateur

#### 14) Modification des identifiants `user.name` et `user.email`
```bash
git config user.name "DevOps Ingenieur"
git config user.email "devops.ingenieur@ihec.ucar.tn"
```

Vérification des nouvelles valeurs actives :
```bash
git config user.name
# DevOps Ingenieur

git config user.email
# devops.ingenieur@ihec.ucar.tn
```

#### 15) Création de `file4.txt`, commit et consultation du log
```bash
echo "Quatrieme fichier pour tester le changement d auteur." > file4.txt
git add file4.txt
git commit -m "Troisieme commit : ajout de file4.txt avec nouvel auteur"
```

**Affichage du log détaillé (`git log -1`) :**
```text
commit 3d99ab935dca330fbcbab38843abcb3c2807fddb (HEAD -> master)
Author: DevOps Ingenieur <devops.ingenieur@ihec.ucar.tn>
Date:   Tue Nov 10 12:53:45 2026 +0100

    Troisieme commit : ajout de file4.txt avec nouvel auteur
```

**Affichage synthétique de l'historique complet avec auteurs :**
```bash
git log --pretty=format:"%h | Auteur: %an <%ae> | Date: %ad | Message: %s" --date=short
```

**Résultat :**
```text
3d99ab9 | Auteur: DevOps Ingenieur <devops.ingenieur@ihec.ucar.tn> | Date: 2026-11-10 | Message: Troisieme commit : ajout de file4.txt avec nouvel auteur
545b620 | Auteur: Molka Jebali <molka.jebali.m2025@ihec.ucar.tn>        | Date: 2026-11-10 | Message: Deuxieme commit : ajout de file2.txt et file3.txt
bab88c3 | Auteur: Molka Jebali <molka.jebali.m2025@ihec.ucar.tn>        | Date: 2026-11-10 | Message: Premier commit : ajout de file1.txt
```

**Constat :**
- Les commits antérieurs conservent leur auteur initial (`Molka Jebali`).
- Le nouveau commit est immédiatement et fidèlement attribué au nouvel utilisateur configuré (`DevOps Ingenieur`).
- Cela démontre le principe d'immuabilité de Git : un commit est cryptographiquement scellé avec les métadonnées présentes au moment précis de sa création.

---

## 4. Tableau récapitulatif des commandes Git maîtrisées dans ce TP

| Commande | Rôle |
|---|---|
| `git config [--global / --local] user.name "Nom"` | Définir le nom de l'auteur des commits |
| `git config [--global / --local] user.email "Email"` | Définir l'adresse email de l'auteur |
| `git init` | Initialiser un nouveau dépôt local Git dans le dossier courant |
| `git status` | Vérifier l'état des fichiers (Untracked, Staged, Clean) |
| `git add <fichier>` | Placer un fichier dans la Staging Area (indexation) |
| `git add .` | Indexer tous les fichiers nouveaux et modifiés du répertoire courant |
| `git commit -m "Message"` | Enregistrer un instantané (snapshot) permanent dans le dépôt |
| `git log` | Afficher l'historique complet des commits |
| `git log -n` | Limiter l'affichage aux `n` derniers commits |
| `git log --oneline` | Afficher l'historique sous forme condensée (1 ligne par commit) |

---
**Conclusion :** Le TP1 est entièrement réalisé et validé. Le dépôt local `TP1` est opérationnel avec son arborescence de fichiers, ses commits traçables et ses métadonnées d'auteurs vérifiées.

