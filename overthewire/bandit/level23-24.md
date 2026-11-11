# Bandit, niveau 23 → 24

## Le but
Encore une tâche cron, mais cette fois il faut écrire mon premier script shell. L'énoncé prévient que le script sera supprimé une fois exécuté, donc il vaut mieux en garder une copie.

## Comment j'ai fait
1. `cat /etc/cron.d/cronjob_bandit24` : la tâche lance `/usr/bin/cronjob_bandit24.sh` en tant que `bandit24`, toutes les minutes.
2. Je lis le script. En résumé, il :
   - va dans `/var/spool/bandit24/foo` ;
   - pour chaque fichier de ce dossier, regarde son propriétaire avec `stat` ;
   - si le propriétaire est `bandit23`, l'exécute (avec un délai max de 60 s via `timeout`) ;
   - le supprime ensuite, qu'il ait été exécuté ou non.
3. Donc : si je dépose dans ce dossier un script qui m'appartient, `bandit24` va l'exécuter avec ses droits. Il me suffit d'écrire un script qui copie `/etc/bandit_pass/bandit24` dans un endroit que je peux lire.
4. Je prépare un dossier de travail que `bandit24` pourra remplir :
   ```bash
   cd "$(mktemp -d)"
   chmod 777 .
   ```
   `chmod 777` parce que c'est `bandit24` qui va écrire le fichier dedans, pas moi.
5. J'écris le script `pass.sh` dans ce dossier :
   ```bash
   #!/bin/bash
   cat /etc/bandit_pass/bandit24 > /tmp/<mon dossier>/password
   ```
6. Je le rends exécutable (`chmod +x pass.sh`), puis je le copie dans `/var/spool/bandit24/foo/`. Copier en tant que `bandit23` fait de moi le propriétaire de la copie, c'est la condition vérifiée par le script de cron.
7. J'attends au plus une minute, puis `cat password` dans mon dossier.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `#!/bin/bash` | Le *shebang* : première ligne qui dit quel interpréteur doit exécuter le fichier. Sans lui, le script peut être lancé par un autre shell. |
| `mktemp -d` | Crée un dossier au nom aléatoire dans `/tmp` (déjà vu au niveau 12), pour ne pas gêner les autres joueurs. |
| `chmod 777 <dossier>` | Lecture, écriture, traversée pour tout le monde. Nécessaire pour que `bandit24` puisse y créer `password`. |
| `chmod +x pass.sh` | Ajoute le droit d'exécution : sans lui, cron ne peut pas lancer le script. |
| `cp pass.sh /var/spool/bandit24/foo/` | Dépose la copie. Le dossier est en écriture pour moi mais pas en lecture (`ls` y donne *Permission denied*), d'où l'intérêt de garder l'original ailleurs. |
| `stat --format "%U" <fichier>` | Dans le script de cron : affiche le propriétaire du fichier. C'est comme ça qu'il filtre ce qu'il exécute. |
| `timeout -s 9 60 ./<fichier>` | Dans le script de cron : tue le script s'il dure plus de 60 s. Mon script doit donc être court. |

## Autres façons de faire
- Écrire le script en une ligne sans éditeur : `echo 'cat /etc/bandit_pass/bandit24 > /tmp/<dossier>/password' > pass.sh`.
- Faire écrire le résultat directement dans `/tmp/<nom unique>` puis le rendre lisible (`chmod 644`) dans le script, au lieu de préparer un dossier en 777.
- Surveiller l'arrivée du fichier sans relancer `cat` à la main : `watch -n 5 ls -l` dans mon dossier.

Si rien n'apparaît après une minute : vérifier que le script est bien dans `foo/` (et pas dans `/var/spool/bandit24/`), qu'il est exécutable, et que le dossier de sortie est bien accessible en écriture à `bandit24`.

## À retenir
- Une tâche planifiée qui exécute des fichiers déposés par d'autres utilisateurs, c'est une escalade de privilèges toute prête : quiconque peut écrire dans ce dossier exécute du code avec les droits du compte de la tâche.
- Les droits d'un dossier : `w` sans `r`, on peut y déposer un fichier sans pouvoir lister le contenu.
- Les bases d'un script shell : shebang, droit d'exécution, redirection vers un fichier.

---
[niveau précédent](level22-23.md) · [sommaire](README.md) · [niveau suivant](level24-25.md)
