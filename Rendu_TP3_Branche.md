# Compte Rendu - TP3 : Git — Gestion des Branches et Stratégies de Versioning

**Module :** DevOps / Gestion de Versions Distribuées  
**Niveau :** Master 2 - Big Data  
**Sujet :** TP3 : Gestion des Branches  
**Date :** 10 Novembre 2026  

---

## 1. Objectifs du TP
- Travailler simultanément avec plusieurs versions isolées du code source sans impacter la version stable de production.
- Créer, lister, basculer (`checkout` / `switch`) et renommer des branches locales et distantes.
- Comprendre le déplacement du pointeur spécial `HEAD` à travers l'arbre des commits.
- Inspecter les écarts de code ligne par ligne et par fichier avec `git diff`.
- Maîtriser les procédures de restauration et de rollback face aux régressions et aux bugs.

---

## 2. Rappel conceptuel : Que représente une branche dans Git ?

Contrairement à d'autres systèmes de gestion de versions qui dupliquent physiquement l'intégralité du code lors d'un embranchement, **une branche dans Git n'est qu'un simple pointeur mobile de 41 octets** (une référence textuelle dans `.git/refs/heads/`) qui référence un commit spécifique. Le pointeur `HEAD` indique simplement la branche sur laquelle vous êtes actuellement positionné :

```
             HEAD -> Branche1
                   |
                [ d53e7c8 ] (Commit sur Branche1 : ajout filebranch1.txt)
               /
[ 645024f ] --+ 
     |
  master
```

---

## 3. Travail Demandé & Réponses Détaillées aux Questions

### 1. Commande « git branch » : Quel est le résultat obtenu ?
```bash
git branch
```
**Résultat obtenu :**
```text
* master
```
**Analyse :**
- La commande liste l'ensemble des branches locales existant dans le dépôt.
- L'astérisque `*` (et la surbrillance verte dans la plupart des terminaux) indique la branche courante active sur laquelle pointe `HEAD` (ici `master`).

---

### 2. Création de la branche « Branche1 »
```bash
git branch Branche1
```
*Fonctionnement :* Git crée un nouveau fichier `.git/refs/heads/Branche1` contenant le même hash de commit que celui sur lequel se trouve `master`. À cette étape, nous n'avons pas encore changé de branche, `HEAD` pointe toujours sur `master`.

---

### 3. Vérification de la création de la nouvelle branche
```bash
git branch
```
**Résultat obtenu :**
```text
  Branche1
* master
```
On constate que `Branche1` existe bien, et que `master` est toujours la branche active.

---

### 4. Bascule vers la branche avec « git checkout »
```bash
git checkout Branche1
```
*(Note moderne : la commande alternative recommandée est `git switch Branche1`).*

**Résultat obtenu :**
```text
Switched to branch 'Branche1'
```

---

### 5. Vérification de l'emplacement actuel du pointeur `HEAD`
```bash
git branch
```
**Résultat obtenu :**
```text
* Branche1
  master
```
```bash
git status
```
**Résultat obtenu :**
```text
On branch Branche1
nothing to commit, working tree clean
```
**Analyse :** Le pointeur `HEAD` a été mis à jour et référence désormais la branche `Branche1`. Toute future modification ou commit sera rattaché à `Branche1`.

---

### 6. Création de fichier sur « Branche1 », commit, bascule sur « master » et comparaison
1. Création et commit sur `Branche1` :
```bash
echo "Contenu specifique a la branche Branche1" > filebranch1.txt
git add filebranch1.txt
git commit -m "Commit sur Branche1 : ajout de filebranch1.txt"
```
**Sortie du commit :**
```text
[Branche1 d53e7c8] Commit sur Branche1 : ajout de filebranch1.txt
 1 file changed, 1 insertion(+)
 create mode 100644 filebranch1.txt
```

2. Consultation du log sur `Branche1` :
```bash
git log --oneline
```
```text
d53e7c8 (HEAD -> Branche1) Commit sur Branche1 : ajout de filebranch1.txt
645024f (origin/master, master) Commit initial sur master
```

3. Bascule vers la branche principale `master` :
```bash
git checkout master
```
```text
Switched to branch 'master'
```

4. Consultation des commits et des fichiers sur `master` :
```bash
git log --oneline
```
```text
645024f (HEAD -> master, origin/master) Commit initial sur master
```
```bash
dir
# ou ls -la
```
```text
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/10/2026 12:56 PM             30 base.txt
```

**Que remarquez-vous ? (Analyse clé)**
- Le commit `d53e7c8` et le fichier `filebranch1.txt` **ont totalement disparu du répertoire de travail et de l'historique de la branche `master`** !
- **Explication :** Git assure une **isolation stricte** des branches. Tant qu'aucune opération de fusion (`git merge`) ou de rebasage (`git rebase`) n'est exécutée, les travaux d'une branche fonctionnelle n'altèrent en rien la branche principale `master`.

---

### 7. Envoi de la nouvelle branche vers le serveur distant (« git push origin Branche1 »)
```bash
git checkout Branche1
git push origin Branche1
```
**Résultat obtenu :**
```text
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To C:/Users/Lenovo/Desktop/M2-BigData/Devops/TPs/TP3/remote_tp3.git
 * [new branch]      Branche1 -> Branche1
```

Vérification des branches locales et distantes :
```bash
git branch -a
```
```text
* Branche1
  Feature-Auth
  master
  remotes/origin/Branche1
  remotes/origin/master
```

**Que remarquez-vous ?**
- Git a créé une nouvelle référence distante `remotes/origin/Branche1`.
- La branche est maintenant hébergée sur le serveur distant et partagée avec toute l'équipe. En ajoutant l'option `-u` (`git push -u origin Branche1`), Git lie la branche locale à la branche distante (tracking branch).

---

### 8. Modification de fichier et consultation avec « git diff NomFichier »
Ajout d'une ligne dans `filebranch1.txt` :
```bash
echo "Deuxieme ligne ajoutee pour tester git diff." >> filebranch1.txt
git diff filebranch1.txt
```

**Résultat affiché par `git diff` :**
```diff
diff --git a/filebranch1.txt b/filebranch1.txt
index 6286fc3..e4fd75d 100644
--- a/filebranch1.txt
+++ b/filebranch1.txt
@@ -1 +1,2 @@
 Contenu specifique a la branche Branche1
+Deuxieme ligne ajoutee pour tester git diff.
```

**Explication de la sortie `diff` :**
- `--- a/...` : Représente la version du fichier dans l'index / dernier commit.
- `+++ b/...` : Représente la version modifiée dans l'espace de travail.
- `@@ -1 +1,2 @@` : Indique le numéro de ligne d'origine et le nombre de lignes modifiées.
- La ligne préfixée par `+` (en vert) montre la ligne ajoutée. Si une ligne avait été effacée, elle serait préfixée par `-` (en rouge).

---

### 9. Comparaison entre deux commits avec « git diff id1 id2 »
Enregistrons les modifications dans un commit, puis créons un nouveau fichier `file_extra.txt` commité :
```bash
git add filebranch1.txt
git commit -m "Deuxieme commit sur Branche1 : modification de filebranch1.txt"

echo "Autre fichier cree pour le diff" > file_extra.txt
git add file_extra.txt
git commit -m "Troisieme commit sur Branche1 : ajout de file_extra.txt"
```

Pour comparer le deuxième commit (`7c06ab9`) et le troisième commit (`1e46713`) :
```bash
git diff 7c06ab9 1e46713
```
**Résultat obtenu :**
```diff
diff --git a/file_extra.txt b/file_extra.txt
new file mode 100644
index 0000000..4e74987
--- /dev/null
+++ b/file_extra.txt
@@ -0,0 +1 @@
+Autre fichier cree pour le diff
```

---

### 10. Consultation des fichiers modifiés entre deux commits (« git diff id1 id2 --name-only »)
```bash
git diff 7c06ab9 1e46713 --name-only
```
**Résultat obtenu :**
```text
file_extra.txt
```
*Explication :* L'option `--name-only` affiche uniquement le ou les noms des fichiers modifiés, ajoutés ou supprimés entre deux révisions, sans encombrer la console avec le contenu ligne par ligne.

---

### Étude de cas / Mise en situation réelle :
> **Problématique posée :**  
> *"Supposons qu’on a 15 copies de code source chez les développeurs. Un parmi ces développeurs a créé une branche à part, et a codé sa partie avec plusieurs modifications en local, il a trouvé des bugs qu’il n’a pas pu les résoudre. Quelle est la solution !!"*

#### Analyse approfondie et solutions professionnelles DevOps :

Cette situation illustre l'un des plus grands avantages des branches Git : **le droit à l'erreur sans risque pour l'équipe**. Plusieurs cas se présentent selon le stade d'avancement du développeur :

1. **Cas 1 : Les modifications contenant des bugs ne sont pas encore commitées (fichiers modifiés dans le Working Directory) :**
   - **Solution :** Annuler immédiatement toutes les modifications en cours pour revenir à l'état propre du dernier commit :
     ```bash
     git checkout -- .
     # Ou syntaxe moderne Git 2.23+ :
     git restore .
     ```
   - Pour supprimer également les nouveaux fichiers non suivis créés pendant les tests :
     ```bash
     git clean -fd
     ```

2. **Cas 2 : Le développeur a effectué des commits locaux défectueux, mais souhaite repartir d'un commit antérieur stable :**
   - **Solution :** Réinitialiser la branche locale sur le commit stable (ou sur l'état officiel distant) :
     ```bash
     git reset --hard HEAD~1
     # Ou directement caler sa branche sur le master distant propre :
     git reset --hard origin/master
     ```

3. **Cas 3 : La branche expérimentale est devenue totalement irrécupérable et doit être abandonnée :**
   - **Solution :** Revenir sur la branche principale propre et détruire purement et simplement la branche locale défectueuse :
     ```bash
     git checkout master
     git branch -D nom-de-la-branche-buggee
     ```
   - Ensuite, recréer une branche saine à partir de la version propre à jour :
     ```bash
     git pull origin master
     git checkout -b nouvelle-branche-saine
     ```

4. **Cas 4 : Le développeur veut conserver ses essais pour les montrer à un collègue ou les analyser plus tard sans bloquer son travail :**
   - **Solution A (Mise en réserve temporaire) :**
     ```bash
     git stash
     ```
   - **Solution B (Branche d'investigation) :** Renommer la branche en `debug/recherche-bug` et créer une nouvelle branche pour repartir de zéro.

---

### 11. Test de Rollback avec « git checkout -- NomFichier »

1. Création d'un fichier de test et commit :
```bash
echo "Contenu initial du fichier rollback" > test_rollback.txt
git add test_rollback.txt
git commit -m "Commit de test_rollback.txt"
```

2. Altération accidentelle ou bug :
```bash
echo "MODIFICATION ACCIDENTELLE OU BUGUEE" > test_rollback.txt
cat test_rollback.txt
# Affiche : MODIFICATION ACCIDENTELLE OU BUGUEE
```

3. Exécution du rollback :
```bash
git checkout -- test_rollback.txt
# Ou : git restore test_rollback.txt
```

4. Vérification de la restauration :
```bash
cat test_rollback.txt
# Affiche : Contenu initial du fichier rollback
```
**Conclusion du test :** Le fichier a été instantanément restauré à son état exact lors du dernier commit.

---

### Commandes bonus indispensables

#### Créer et basculer sur une branche en une seule commande :
```bash
git checkout -b feature-authentification
# Ou commande moderne :
git switch -c feature-authentification
```

#### Renommer une branche :
```bash
git branch -m feature-authentification Feature-Auth
```
Vérification :
```bash
git branch
```
```text
  Branche1
* Feature-Auth
  master
```

---

## 4. Tableau récapitulatif des commandes de gestion de branches

| Commande | Action |
|---|---|
| `git branch` | Lister les branches locales (`*` = branche courante) |
| `git branch <nom>` | Créer une nouvelle branche locale sans s'y déplacer |
| `git checkout <nom>` ou `git switch <nom>` | Se positionner sur une branche existante |
| `git checkout -b <nom>` ou `git switch -c <nom>` | Créer une branche et basculer dessus immédiatement |
| `git branch -m <ancien> <nouveau>` | Renommer une branche locale |
| `git branch -d <nom>` / `-D <nom>` | Supprimer une branche (sécurisé `-d`, forcée `-D`) |
| `git push origin <nom>` | Publier une branche locale vers le repository distant |
| `git diff <fichier>` | Voir les modifications non indexées ligne par ligne |
| `git diff <c1> <c2>` | Comparer les modifications complètes entre deux commits |
| `git diff <c1> <c2> --name-only` | Lister uniquement les noms des fichiers modifiés entre deux commits |
| `git checkout -- <fichier>` ou `git restore <fichier>` | Annuler les modifications non commitées d'un fichier (rollback) |
| `git checkout -- .` ou `git restore .` | Annuler toutes les modifications non commitées du répertoire |

---
**Conclusion :** Le TP3 est validé avec succès. Les concepts d'isolation des branches, de navigation de pointeurs, d'inspection différentielle (`diff`) et de gestion des incidents / rollbacks sont pleinement assimilés.

