# Bandit, niveau 13 → 14

## Le but
Pas de mot de passe cette fois : une clé privée SSH permet de se connecter en tant que `bandit14`, dont le mot de passe est lisible uniquement par `bandit14`.

## Comment j'ai fait
1. Le home contient `sshkey.private`. Une clé privée remplace le mot de passe pour s'authentifier en SSH.
2. La connexion SSH vers `localhost` depuis le serveur étant bloquée, je récupère la clé sur ma machine avec `scp`.
3. SSH refuse une clé privée lisible par d'autres utilisateurs : il faut restreindre ses permissions.
4. Je me connecte en `bandit14` avec la clé, puis je peux lire le fichier de mot de passe de `bandit14`.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .` | *Secure copy* : copie un fichier à travers SSH. ⚠️ Ici le port s'écrit `-P` majuscule (contrairement à `ssh -p`). `.` = dossier courant local. |
| `chmod 600 sshkey.private` | Lecture/écriture pour le propriétaire seulement. Sinon SSH affiche `UNPROTECTED PRIVATE KEY FILE` et ignore la clé. |
| `ssh-keygen -y -f sshkey.private` | Recalcule la clé publique à partir de la clé privée : bon moyen de vérifier que le fichier est valide et complet. |
| `ssh -i sshkey.private bandit14@... -p 2220` | `-i` (*identity file*) indique quelle clé utiliser pour l'authentification. |

## Autres façons de faire
- Afficher la clé avec `cat`, la copier-coller dans un fichier local (attention aux lignes `-----BEGIN/END-----` qui doivent être complètes).
- `sftp -P 2220 bandit13@...` puis `get sshkey.private`.
- Sous Windows, `chmod` n'existe pas : restreindre les droits avec `icacls sshkey.private /inheritance:r /grant:r "%USERNAME%:R"`, ou faire l'exercice depuis WSL.

## À retenir
- Authentification SSH par clé (paire publique/privée).
- Pourquoi les permissions d'une clé privée sont critiques.
- `scp` / `sftp` pour transférer des fichiers via SSH.

---
[niveau précédent](level12-13.md) · [sommaire](README.md) · [niveau suivant](level14-15.md)
