# Bandit, niveau 3 → 4

## Le but
Trouver un fichier **caché** dans le dossier `inhere` et le lire.

## Comment j'ai fait
1. `ls inhere` n'affiche rien : le dossier semble vide.
2. Sous Linux, un fichier dont le nom **commence par un point** est caché par convention : `ls` ne l'affiche pas par défaut.
3. J'affiche donc tous les fichiers, puis je lis celui qui apparaît.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `ls -a inhere` | `-a` (*all*) affiche aussi les entrées commençant par `.`, y compris `.` (dossier courant) et `..` (dossier parent). |
| `ls -la inhere` | `-l` ajoute le format long : permissions, propriétaire, taille, date. |
| `cat inhere/<nom>` | Affiche le fichier trouvé (penser aux guillemets ou à `./` si le nom contient des caractères spéciaux). |

## Autres façons de faire
- `ls -A` : comme `-a` mais sans `.` et `..`.
- `find inhere -type f` : liste tous les fichiers, cachés ou non, sans se soucier de la convention du point.
- `find inhere -type f -exec cat {} \;` : trouve et affiche en une seule commande.

## À retenir
- « Caché » sous Linux n'est **pas** une protection : c'est juste une convention d'affichage.
- `find` ignore cette convention, c'est souvent l'outil le plus fiable pour explorer.

---
[niveau précédent](level2-3.md) · [sommaire](README.md) · [niveau suivant](level4-5.md)
