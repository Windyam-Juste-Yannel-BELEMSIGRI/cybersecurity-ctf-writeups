# Bandit, niveau 1 → 2

## Le but
Lire un fichier dont le nom est simplement `-` (un tiret).

## Comment j'ai fait
1. `ls` montre un fichier nommé `-`.
2. `cat -` ne marche pas : la commande reste bloquée en attente. Par **convention Unix**, un argument `-` signifie « lire l'entrée standard (le clavier) » et non « le fichier nommé tiret ».
3. Il faut donc trouver un moyen de passer le fichier à `cat` sans qu'il soit interprété comme ce symbole spécial.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `cat ./-` | `./` = « dans le répertoire courant ». L'argument devient un **chemin** et n'est plus le symbole `-`. |
| `cat < -` | `<` est une **redirection du shell** : c'est bash qui ouvre le fichier `-` et le branche sur l'entrée standard de `cat`. `cat` ne reçoit aucun argument, il ne voit donc jamais le `-`. |
| `more -` | `more` ne traite pas `-` comme l'entrée standard de la même façon, il ouvre le fichier. Ça marche, mais c'est un comportement propre à l'outil, donc moins fiable. |

## Autres façons de faire
- `cat "$PWD/-"` ou `cat ~/-` : chemin absolu.
- `less ./-`, `head ./-`, `tail ./-`.
- ⚠️ Piège : `cat -- -` **ne marche pas**. `--` signale la fin des options, mais `-` reste « entrée standard » pour `cat`.

## À retenir
- Les noms de fichiers « spéciaux » et la convention `-` = stdin.
- La différence entre un **argument** passé à une commande et une **redirection** gérée par le shell.

---
[niveau précédent](level0-1.md) · [sommaire](README.md) · [niveau suivant](level2-3.md)
