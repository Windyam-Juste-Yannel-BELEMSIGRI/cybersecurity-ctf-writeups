# Bandit, niveau 11 → 12

## Le but
`data.txt` est chiffré en **ROT13** : chaque lettre est décalée de 13 positions dans l'alphabet.

## Comment j'ai fait
1. ROT13 est un chiffre de César avec un décalage de 13. Comme l'alphabet a 26 lettres, appliquer ROT13 deux fois redonne le texte d'origine : **chiffrer = déchiffrer**.
2. Il suffit de remplacer chaque lettre par celle située 13 rangs plus loin, ce que fait `tr` avec des plages de caractères.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt` | `tr` (*translate*) remplace caractère par caractère : le 1er caractère du 1er ensemble devient le 1er du 2ᵉ, etc. Ici `A→N`, `B→O`, …, `M→Z`, `N→A`… Les chiffres et symboles ne sont pas touchés. |
| `cat data.txt \| tr ...` | Variante avec pipe : `tr` ne lit **que** l'entrée standard, il n'accepte pas de nom de fichier, d'où le `cat` ou le `<`. |

## Autres façons de faire
- `python3 -c "import codecs; print(codecs.decode(open('data.txt').read(), 'rot13'))"`
- `sed 'y/ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz/NOPQRSTUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm/' data.txt`
- CyberChef, opération « ROT13 ».

## À retenir
- `tr` pour les substitutions caractère par caractère (aussi utile pour `tr 'a-z' 'A-Z'`, ou `tr -d` pour supprimer des caractères).
- Les chiffrements par substitution simples ne protègent rien.

---
[niveau précédent](level10-11.md) · [sommaire](README.md) · [niveau suivant](level12-13.md)
