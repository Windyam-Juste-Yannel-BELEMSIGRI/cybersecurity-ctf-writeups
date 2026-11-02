# Bandit, niveau 9 → 10

## Le but
Dans `data.txt` (majoritairement binaire), trouver l'une des rares chaînes lisibles, **précédée de plusieurs `=`**.

## Comment j'ai fait
1. `cat` sur un fichier binaire affiche des caractères illisibles et `grep` répond juste « binary file matches ».
2. J'extrais d'abord uniquement les morceaux de texte lisibles, puis je filtre ceux qui contiennent des `=`.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `strings data.txt` | Extrait d'un fichier binaire toutes les suites d'au moins 4 caractères imprimables. Très utilisé pour analyser des exécutables ou des fichiers inconnus. Il ne « rend pas le fichier lisible », il en **extrait** la partie texte. |
| `\| grep '==='` | Ne garde que les chaînes qui contiennent `===`. Mettre le motif entre quotes est une bonne habitude. |

## Autres façons de faire
- `grep -a '===' data.txt` : `-a` force `grep` à traiter le binaire comme du texte.
- `strings -n 10 data.txt` : ne garder que les chaînes d'au moins 10 caractères, ce qui réduit le bruit.
- `xxd data.txt | less` ou `hexdump -C data.txt` : inspection hexadécimale manuelle.

## À retenir
- `strings` est un outil de base en analyse de binaires et en forensic.

---
[niveau précédent](level8-9.md) · [sommaire](README.md) · [niveau suivant](level10-11.md)
