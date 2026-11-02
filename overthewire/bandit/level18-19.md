# Bandit, niveau 18 → 19

## Le but
Le mot de passe est dans `readme`, mais le `.bashrc` a été modifié pour **déconnecter** dès la connexion SSH.

## Comment j'ai fait
1. `.bashrc` est un script exécuté automatiquement par **bash** au démarrage d'un shell interactif. Ici il contient un `exit`.
2. Je n'ai pas besoin d'un shell interactif bash : je peux soit demander un **autre shell** qui ne lit pas `.bashrc`, soit demander à SSH d'**exécuter directement une commande**.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `ssh -t bandit18@... -p 2220 /bin/sh` | Ce qui suit l'hôte est la commande à lancer à la place du shell par défaut. `/bin/sh` (souvent *dash*) ne lit pas `.bashrc`, donc pas de déconnexion. `-t` force l'allocation d'un **pseudo-terminal**, indispensable pour avoir un shell interactif utilisable. |
| `cat readme` | Une fois dans `sh`, on lit le fichier normalement. |

## Autres façons de faire
- `ssh bandit18@... -p 2220 cat readme` : le plus direct. SSH exécute la commande, affiche le résultat et ferme la connexion, sans session interactive.
- `ssh -t bandit18@... -p 2220 bash --norc` : bash, mais sans lire `.bashrc`.
- `scp -P 2220 bandit18@...:readme .` : copier le fichier sur sa machine.

## À retenir
- Le rôle des fichiers de démarrage du shell (`.bashrc`, `.profile`) et leurs limites.
- SSH peut exécuter une commande distante unique, très utile en scripts et en automatisation.

---
[niveau précédent](level17-18.md) · [sommaire](README.md) · [niveau suivant](level19-20.md)
