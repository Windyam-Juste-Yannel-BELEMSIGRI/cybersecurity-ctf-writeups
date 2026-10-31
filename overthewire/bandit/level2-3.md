# Bandit — Niveau 2 → 3

## 🎯 Objectif
Lire un fichier dont le nom contient des espaces et commence par `--`.

## 🧭 Cheminement
1. Deux problèmes se cumulent :
   - les **espaces** : le shell découpe la ligne de commande sur les espaces, donc le nom serait vu comme plusieurs arguments ;
   - le **`--` au début** : `cat` croit recevoir une option longue (comme `--help`).
2. Il faut donc (a) garder le nom en un seul morceau et (b) empêcher `cat` de le lire comme une option.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `"..."` (guillemets) | Indiquent au shell de traiter tout le texte comme **un seul argument**, espaces compris. |
| `cat < "--spaces in this filename--"` | La redirection fait ouvrir le fichier par le shell, `cat` ne voit jamais le nom : règle les deux problèmes. |
| `cat ./"--spaces in this filename--"` | Le préfixe `./` fait commencer l'argument par `.` : ce n'est plus une option. |
| `cat -- "--spaces in this filename--"` | `--` = « fin des options » (convention POSIX). Tout ce qui suit est traité comme un nom de fichier. |

## 🔁 Alternatives possibles
- **Touche Tab** : l'auto-complétion échappe automatiquement les espaces (`--spaces\ in\ this\ filename--`).
- Échapper chaque espace avec `\` à la main.
- `more`/`less` avec les mêmes astuces (`./` ou `<`).

## 📚 Ce que j'ai appris
- Comment le shell découpe les arguments (*word splitting*) et comment l'en empêcher (guillemets, `\`).
- La convention `--` pour arrêter l'analyse des options, utile avec n'importe quelle commande.

---
[← Niveau précédent](level1-2.md) · [Sommaire](README.md) · [Niveau suivant →](level3-4.md)
