# Bandit, niveau 12 → 13

## Le but
`data.txt` est le hexdump d'un fichier compressé plusieurs fois avec différents formats. Remonter jusqu'au texte d'origine.

## Comment j'ai fait
1. Le home n'est pas modifiable : je crée un dossier de travail temporaire et j'y copie le fichier.
2. Un hexdump est une représentation texte des octets ; il faut d'abord reconstruire le fichier binaire.
3. Ensuite, c'est une boucle :
   - `file` me dit quel est le format actuel (gzip, bzip2, tar…) ;
   - je renomme avec la bonne extension si l'outil l'exige ;
   - je décompresse ;
   - je recommence jusqu'à obtenir « ASCII text ».

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `mktemp -d` | Crée un répertoire au nom aléatoire dans `/tmp` et affiche son chemin. Évite les collisions avec les autres joueurs. |
| `cp` / `mv` | Copier / déplacer ou renommer un fichier. |
| `xxd -r data.txt data.bin` | `xxd` fabrique un hexdump ; `-r` (*reverse*) fait l'inverse : texte hexa → octets bruts. |
| `file data.bin` | Identifie le format grâce aux *magic bytes* (`1f 8b` = gzip, `42 5a 68` = bzip2 « BZh »…). |
| `gunzip` / `gzip -d` | Décompresse du gzip. `gunzip` refuse un fichier sans extension `.gz`, d'où le `mv` avant. |
| `bzip2 -d` | Décompresse du bzip2. |
| `tar -xf` | Extrait une archive tar. `tar` regroupe plusieurs fichiers en un, il ne compresse pas forcément. `-x` = extraire, `-f` = fichier. |

## Autres façons de faire
- `zcat` / `bzcat` : décompressent vers la sortie standard, sans se soucier de l'extension.
- `7z x fichier` : sait ouvrir presque tous les formats.
- `tar -xzf` / `tar -xjf` : extraction + décompression gzip/bzip2 en une seule étape.
- Automatiser la boucle avec un petit script bash qui lit la sortie de `file` et choisit l'outil.

## À retenir
- Différence entre archive (tar) et compression (gzip, bzip2, xz).
- Toujours identifier un fichier avec `file` plutôt que de se fier à son nom.
- Les hexdumps et `xxd`.

---
[niveau précédent](level11-12.md) · [sommaire](README.md) · [niveau suivant](level13-14.md)
