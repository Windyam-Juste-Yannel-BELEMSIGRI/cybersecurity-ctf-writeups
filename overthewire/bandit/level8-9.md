# Bandit — Niveau 8 → 9

## 🎯 Objectif
Dans `data.txt`, trouver la **seule ligne qui n'apparaît qu'une fois**.

## 🧭 Cheminement
1. `uniq` sait supprimer ou repérer les doublons, **mais seulement s'ils sont sur des lignes consécutives**.
2. Il faut donc d'abord trier le fichier pour que les lignes identiques se retrouvent côte à côte.
3. Ensuite, `uniq -u` ne garde que les lignes sans doublon.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `sort data.txt` | Trie les lignes, ce qui regroupe les lignes identiques. |
| `\| uniq -u` | `uniq` compare chaque ligne à la précédente. `-u` (*unique*) n'affiche que les lignes qui n'ont **aucun** voisin identique. (`-d` ferait l'inverse : uniquement les doublons.) |

## 🔁 Alternatives possibles
- `sort data.txt | uniq -c | sort -n | head -1` : `-c` compte les occurrences ; trié numériquement, la ligne à 1 occurrence sort en premier. Plus visuel pour comprendre.
- `awk '{c[$0]++} END {for (l in c) if (c[l]==1) print l}' data.txt` : compte avec un tableau associatif, sans trier.

## 📚 Ce que j'ai appris
- Pourquoi `sort | uniq` est un duo classique.
- Les options `-u`, `-d`, `-c` de `uniq`.

---
[← Niveau précédent](level7-8.md) · [Sommaire](README.md) · [Niveau suivant →](level9-10.md)
