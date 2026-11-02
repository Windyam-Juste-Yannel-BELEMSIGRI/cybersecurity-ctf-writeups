# Bandit, niveau 4 → 5

## Le but
Dans `inhere`, plusieurs fichiers (`-file00`, `-file01`, …) : trouver le **seul lisible par un humain**.

## Comment j'ai fait
1. Ouvrir chaque fichier un par un avec `cat` serait long et pollue le terminal avec des caractères binaires.
2. Il faut un outil qui identifie le **type réel** du contenu d'un fichier : c'est le rôle de `file`.
3. Je lance `file` sur tous les fichiers, je filtre ceux identifiés comme texte, puis je lis le bon.
4. Attention : les noms commencent par `-`, d'où l'intérêt de garder le préfixe `inhere/` ou `./` (voir niveau 1).

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `file inhere/*` | `file` ne regarde **pas l'extension** : il lit les premiers octets (*magic bytes*) et le contenu pour deviner le type (texte ASCII, ELF, image, données…). Le joker `*` est développé par le shell en la liste de tous les fichiers. |
| `\| grep ASCII` | Le *pipe* `\|` envoie la sortie de `file` à `grep`, qui ne garde que les lignes contenant « ASCII ». |
| `cat inhere/<nom>` | Lit le fichier identifié. Comme l'argument commence par `inhere/`, le `-` du nom n'est plus un problème. |

## Autres façons de faire
- `file ./inhere/* | grep text` : plus large (attrape aussi « UTF-8 text »).
- `find inhere -type f -exec file {} + | grep text`.
- Boucle : `for f in ./inhere/*; do echo "$f"; cat "$f"; echo; done` puis repérer à l'œil le seul contenu lisible.
- `strings inhere/*` : n'affiche que les suites de caractères imprimables.

## À retenir
- L'extension d'un fichier ne prouve rien : `file` analyse le contenu.
- Combiner des commandes avec `|` pour filtrer un résultat.

---
[niveau précédent](level3-4.md) · [sommaire](README.md) · [niveau suivant](level5-6.md)
