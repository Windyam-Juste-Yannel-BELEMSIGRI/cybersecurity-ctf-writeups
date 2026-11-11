# Bandit, niveau 7 → 8

## Le but
Le mot de passe est dans `data.txt`, sur la ligne qui contient le mot millionth.

## Comment j'ai fait
1. `data.txt` est énorme (des milliers de lignes) : impossible à lire en entier.
2. Je cherche donc directement la ligne qui contient le mot-clé.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `grep millionth data.txt` | `grep` parcourt le fichier ligne par ligne et n'affiche que celles qui correspondent au motif (une expression régulière). |
| `cat data.txt \| grep millionth` | Même résultat, mais `cat` est inutile : `grep` sait lire un fichier tout seul (*Useless Use of Cat*). |

## Autres façons de faire
- `grep -w millionth data.txt` : mot entier uniquement.
- `awk '/millionth/ {print $2}' data.txt` : affiche seulement la 2ᵉ colonne de la ligne trouvée.
- `sed -n '/millionth/p' data.txt`.
- Dans `less data.txt`, taper `/millionth` pour rechercher.

## À retenir
- `grep` est le réflexe n°1 pour fouiller un gros fichier ou des logs.
- `awk` pour extraire une colonne précise.

---
[niveau précédent](level6-7.md) · [sommaire](README.md) · [niveau suivant](level8-9.md)
