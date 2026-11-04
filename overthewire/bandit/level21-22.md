# Bandit, niveau 21 → 22

## Le but
Un programme tourne automatiquement à intervalle régulier grâce à **cron**, le planificateur de tâches. Il faut aller voir sa configuration dans `/etc/cron.d/` et comprendre quelle commande il exécute.

## Comment j'ai fait
1. Le niveau parle de cron, donc je regarde les tâches planifiées du système : `ls -l /etc/cron.d/`. Il y a un fichier par niveau, celui qui m'intéresse est `cronjob_bandit22`.
2. Je le lis. Il lance `/usr/bin/cronjob_bandit22.sh` **en tant que `bandit22`**, au démarrage (`@reboot`) et toutes les minutes (`* * * * *`).
3. Je lis le script. Il fait deux choses : il rend lisible par tout le monde un fichier au nom bizarre dans `/tmp` (`chmod 644`), puis il y copie le contenu de `/etc/bandit_pass/bandit22`.
4. Comme le script tourne avec les droits de `bandit22`, il peut lire ce mot de passe, et comme il rend le fichier lisible par tous, moi en `bandit21` je peux le lire. Il suffit de faire `cat` sur le fichier de `/tmp` indiqué dans le script.

L'idée du niveau : je n'ai pas besoin d'exploiter quoi que ce soit, il suffit de **lire** ce que fait une tâche automatique qui tourne avec plus de droits que moi.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `ls -l /etc/cron.d/` | Liste les fichiers de tâches planifiées du système. Chaque fichier contient une ou plusieurs lignes de crontab. |
| `cat /etc/cron.d/cronjob_bandit22` | Affiche la tâche. Format d'une ligne : `minute heure jour mois jour_semaine utilisateur commande`. Dans `/etc/cron.d/` il y a une colonne **utilisateur** en plus, absente des crontabs perso. |
| `* * * * *` | Les 5 champs de temps : `*` = « toutes les valeurs », donc toutes les minutes. `*/5 * * * *` serait toutes les 5 minutes. |
| `@reboot` | Raccourci cron : une fois au démarrage de la machine. |
| `&> /dev/null` | Redirige la sortie standard **et** les erreurs vers le néant : la tâche ne laisse aucune trace à l'écran ni par mail. |
| `cat /usr/bin/cronjob_bandit22.sh` | Lit le script lancé par cron. Il est lisible par tout le monde, c'est la faille : on voit où il écrit. |
| `cat /tmp/<fichier>` | Lit le fichier que le script remplit toutes les minutes. |

## Autres façons de faire
- `grep -r bandit22 /etc/cron.d/` : trouve directement le bon fichier au lieu de tout lister.
- `crontab -l` : affiche la crontab de l'utilisateur courant. Ici elle est vide, les tâches intéressantes sont dans `/etc/cron.d/`. Les autres endroits où chercher sur une vraie machine : `/etc/crontab`, `/etc/cron.daily/`, `/var/spool/cron/crontabs/`.
- `systemctl list-timers` : sur les systèmes récents, les tâches planifiées peuvent aussi être des *timers* systemd plutôt que cron.

## À retenir
- Le format cron et ses 5 champs de temps.
- En sécurité, les tâches cron qui tournent avec un autre utilisateur (souvent root) sont un classique de l'**escalade de privilèges** : si le script est lisible, il peut révéler des chemins ou des secrets ; s'il est **modifiable**, on fait exécuter ce qu'on veut avec ses droits. Toujours regarder `/etc/cron.d/` sur une machine qu'on audite.
- Un fichier « caché » dans `/tmp` avec un nom aléatoire n'est pas protégé si n'importe qui peut lire le script qui donne son nom.

---
[niveau précédent](level20-21.md) · [sommaire](README.md) · [niveau suivant](level22-23.md)
