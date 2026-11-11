# Bandit, niveau 19 → 20

## Le but
Utiliser un binaire setuid présent dans le home pour lire le mot de passe du niveau suivant dans `/etc/bandit_pass`.

## Comment j'ai fait
1. `ls -l` montre un exécutable avec un `s` à la place du `x` dans les droits du propriétaire : c'est le bit setuid.
2. Un programme setuid s'exécute avec les droits de son propriétaire (ici `bandit20`) et non de celui qui le lance.
3. Je le lance sans argument pour lire son mode d'emploi : il exécute la commande qu'on lui donne.
4. Je l'utilise donc pour lancer une commande de lecture du fichier de mot de passe de `bandit20`, qui m'est normalement interdit.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `ls -l` | Le `s` dans `-rwsr-x---` indique le setuid. Le propriétaire et le groupe sont affichés juste après. |
| `./bandit20-do` | `./` est nécessaire car le dossier courant n'est pas dans le `PATH`. Sans argument, le binaire affiche son usage. |
| `./bandit20-do id` | Bon réflexe pour vérifier : `id` affiche `euid=` (UID effectif) = bandit20, preuve que le setuid fonctionne. |
| `./bandit20-do cat /etc/bandit_pass/bandit20` | `cat` est lancé avec les droits de bandit20, donc la lecture est autorisée. |

## Autres façons de faire
- `./bandit20-do sh -p` : ouvre un shell qui conserve l'UID effectif (`-p` = mode privilégié ; sans lui, `sh`/`bash` abandonnent les droits setuid).
- `stat bandit20-do` pour voir les permissions en octal (`4750` : le `4` = setuid).
- Lister tous les binaires setuid d'un système : `find / -perm -4000 -type f 2>/dev/null`.

## À retenir
- Le bit setuid (et ses cousins setgid `2000` et sticky bit `1000`).
- C'est un vecteur classique d'escalade de privilèges : un binaire setuid mal conçu (qui exécute n'importe quelle commande) donne les droits de son propriétaire. La recherche de binaires setuid est une étape standard d'un audit Linux (cf. GTFOBins).

---
[niveau précédent](level18-19.md) · [sommaire](README.md) · [niveau suivant](level20-21.md)
