# Bandit — Niveau 1 → 2

## 🎯 Objectif
Lire un fichier dont le nom est simplement `-` (un tiret).

## 🧭 Cheminement
1. `ls` montre un fichier nommé `-`.
2. `cat -` ne marche pas : la commande reste bloquée en attente. Par **convention Unix**, un argument `-` signifie « lire l'entrée standard (le clavier) » et non « le fichier nommé tiret ».
3. Il faut donc trouver un moyen de passer le fichier à `cat` sans qu'il soit interprété comme ce symbole spécial.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `cat ./-` | `./` = « dans le répertoire courant ». L'argument devient un **chemin** et n'est plus le symbole `-`. |
| `cat < -` | `<` est une **redirection du shell** : c'est bash qui ouvre le fichier `-` et le branche sur l'entrée standard de `cat`. `cat` ne reçoit aucun argument, il ne voit donc jamais le `-`. |
| `more -` | `more` ne traite pas `-` comme l'entrée standard de la même façon, il ouvre le fichier. Ça marche, mais c'est un comportement propre à l'outil, donc moins fiable. |

## 🔁 Alternatives possibles
- `cat "$PWD/-"` ou `cat ~/-` : chemin absolu.
- `less ./-`, `head ./-`, `tail ./-`.
- ⚠️ Piège : `cat -- -` **ne marche pas**. `--` signale la fin des options, mais `-` reste « entrée standard » pour `cat`.

## 📚 Ce que j'ai appris
- Les noms de fichiers « spéciaux » et la convention `-` = stdin.
- La différence entre un **argument** passé à une commande et une **redirection** gérée par le shell.

---
[← Niveau précédent](level0-1.md) · [Sommaire](README.md) · [Niveau suivant →](level2-3.md)
