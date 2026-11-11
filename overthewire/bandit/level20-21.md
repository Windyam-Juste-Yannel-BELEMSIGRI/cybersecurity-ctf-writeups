# Bandit, niveau 20 → 21

## Le but
Un binaire setuid (`suconnect`) se connecte au port qu'on lui indique sur `localhost`, lit une ligne, et si c'est le mot de passe du niveau actuel, renvoie le mot de passe suivant.

## Comment j'ai fait
1. Le binaire est un client : il faut donc qu'un serveur écoute déjà sur un port de mon choix, prêt à lui envoyer le mot de passe actuel.
2. Je dois faire tourner deux choses en même temps : un serveur en écoute (`nc -l`) et le binaire. Deux options : deux terminaux (deux sessions SSH, ou `tmux`), ou mettre le serveur en arrière-plan avec `&`.
3. Ordre :
   - lancer `nc` en écoute sur un port libre, en lui donnant le mot de passe actuel à envoyer ;
   - lancer `./suconnect <port>` ;
   - `suconnect` reçoit le mot de passe, le valide et renvoie le suivant dans la connexion, ce qui s'affiche côté `nc`.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `nc -l -p <port> < /etc/bandit_pass/bandit20 &` | `-l` = mode écoute (serveur). `-p` = port (selon la version de nc, `nc -l <port>` suffit). La redirection `<` fait envoyer le contenu du fichier au premier client connecté. `&` lance la commande en arrière-plan et rend la main. |
| `./suconnect <port>` | Se connecte au port, lit la ligne envoyée, la compare, et répond. |
| `jobs` / `fg` | Lister les tâches en arrière-plan / ramener une tâche au premier plan. |

## Autres façons de faire
- tmux : `tmux`, puis `Ctrl+B` `%` pour couper l'écran en deux : `nc -l` d'un côté, `./suconnect` de l'autre. Plus lisible pour comprendre ce qui se passe.
- Deux sessions SSH ouvertes en parallèle.
- `echo -n "$(cat /etc/bandit_pass/bandit20)" | nc -l -p <port> &`.
- `socat TCP-LISTEN:<port> FILE:/etc/bandit_pass/bandit20 &`.

## À retenir
- La différence client/serveur : `nc` peut jouer les deux rôles.
- La gestion des tâches (*job control*) en bash : `&`, `jobs`, `fg`, `bg`, `Ctrl+Z`.

---
[niveau précédent](level19-20.md) · [sommaire](README.md) · [niveau suivant](level21-22.md)
