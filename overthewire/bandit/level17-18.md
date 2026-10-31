# Bandit — Niveau 17 → 18

## 🎯 Objectif
Le mot de passe est **la seule ligne modifiée** entre `passwords.old` et `passwords.new`.

## 🧭 Cheminement
1. Les deux fichiers sont presque identiques : il faut un outil qui compare ligne par ligne.
2. `diff` affiche les différences ; la ligne qui vient de `passwords.new` est la bonne.
3. Note : en se connectant ensuite au niveau 18, on est immédiatement déconnecté avec « Byebye! ». C'est normal, c'est le sujet du niveau suivant.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `ssh -i <clé> bandit17@... -p 2220` | Connexion avec la clé récupérée au niveau 16. |
| `diff passwords.old passwords.new` | Compare les deux fichiers. Dans la sortie, `<` = ligne du **1er** fichier (old), `>` = ligne du **2ᵉ** fichier (new). `5c5` = « la ligne 5 a été changée ». |

## 🔁 Alternatives possibles
- `diff --color` ou `diff -u` (format *unified*, celui de git).
- `grep -vxFf passwords.old passwords.new` : affiche les lignes de `new` absentes de `old` (`-F` texte littéral, `-x` ligne entière, `-f` motifs lus depuis un fichier, `-v` inverser).
- `comm -13 <(sort passwords.old) <(sort passwords.new)` : lignes uniquement dans le second fichier.
- `vimdiff passwords.old passwords.new` : comparaison visuelle.

## 📚 Ce que j'ai appris
- Lire la sortie de `diff` (`<`, `>`, `c`/`a`/`d`).
- La substitution de processus `<(...)` en bash.

---
[← Niveau précédent](level16-17.md) · [Sommaire](README.md) · [Niveau suivant →](level18-19.md)
