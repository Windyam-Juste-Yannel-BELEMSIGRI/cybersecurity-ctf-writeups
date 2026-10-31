# Bandit — Niveau 5 → 6

## 🎯 Objectif
Trouver, quelque part sous `inhere` (avec beaucoup de sous-dossiers), le fichier qui est à la fois **lisible par un humain**, **de 1033 octets** et **non exécutable**.

## 🧭 Cheminement
1. Il y a trop de dossiers pour chercher à la main : c'est un travail pour `find`, qui sait combiner plusieurs critères.
2. Je traduis chaque propriété de l'énoncé en option de `find` (taille, type, permission).
3. Si plusieurs fichiers ressortaient, j'ajouterais un contrôle avec `file` pour le critère « lisible ».

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `find inhere -type f` | Parcourt récursivement l'arborescence. `-type f` = fichiers ordinaires seulement (pas les dossiers). |
| `-size 1033c` | Taille exacte. ⚠️ Le suffixe **`c` = octets** (*characters*). Sans suffixe, l'unité par défaut est le **bloc de 512 octets**, une erreur classique. `+N` = plus grand, `-N` = plus petit. |
| `! -executable` ou `-not -executable` | `!` inverse le test qui suit. `-executable` teste si l'utilisateur courant a le droit d'exécution. |

## 🔁 Alternatives possibles
- Ajouter le critère « lisible » directement : `find inhere -type f -size 1033c ! -executable -exec file {} + | grep text`.
- `ls -laR inhere | grep 1033` : approche plus brute (liste récursive puis filtre sur la taille).
- `du -ab inhere | grep 1033` : `du -b` affiche la taille apparente en octets.

## 📚 Ce que j'ai appris
- `find` est l'outil central de la recherche de fichiers : taille, type, permissions, propriétaire, date…
- Les unités de `-size` (`c`, `k`, `M`, défaut = blocs de 512).

---
[← Niveau précédent](level4-5.md) · [Sommaire](README.md) · [Niveau suivant →](level6-7.md)
