# Bandit — Niveau 9 → 10

## 🎯 Objectif
Dans `data.txt` (majoritairement binaire), trouver l'une des rares chaînes lisibles, **précédée de plusieurs `=`**.

## 🧭 Cheminement
1. `cat` sur un fichier binaire affiche des caractères illisibles et `grep` répond juste « binary file matches ».
2. J'extrais d'abord uniquement les morceaux de texte lisibles, puis je filtre ceux qui contiennent des `=`.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `strings data.txt` | Extrait d'un fichier binaire toutes les suites d'au moins 4 caractères imprimables. Très utilisé pour analyser des exécutables ou des fichiers inconnus. Il ne « rend pas le fichier lisible », il en **extrait** la partie texte. |
| `\| grep '==='` | Ne garde que les chaînes qui contiennent `===`. Mettre le motif entre quotes est une bonne habitude. |

## 🔁 Alternatives possibles
- `grep -a '===' data.txt` : `-a` force `grep` à traiter le binaire comme du texte.
- `strings -n 10 data.txt` : ne garder que les chaînes d'au moins 10 caractères, ce qui réduit le bruit.
- `xxd data.txt | less` ou `hexdump -C data.txt` : inspection hexadécimale manuelle.

## 📚 Ce que j'ai appris
- `strings` est un outil de base en analyse de binaires et en forensic.

---
[← Niveau précédent](level8-9.md) · [Sommaire](README.md) · [Niveau suivant →](level10-11.md)
