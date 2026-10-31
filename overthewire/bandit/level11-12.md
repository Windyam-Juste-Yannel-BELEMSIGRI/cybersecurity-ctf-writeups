# Bandit — Niveau 11 → 12

## 🎯 Objectif
`data.txt` est chiffré en **ROT13** : chaque lettre est décalée de 13 positions dans l'alphabet.

## 🧭 Cheminement
1. ROT13 est un chiffre de César avec un décalage de 13. Comme l'alphabet a 26 lettres, appliquer ROT13 deux fois redonne le texte d'origine : **chiffrer = déchiffrer**.
2. Il suffit de remplacer chaque lettre par celle située 13 rangs plus loin, ce que fait `tr` avec des plages de caractères.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt` | `tr` (*translate*) remplace caractère par caractère : le 1er caractère du 1er ensemble devient le 1er du 2ᵉ, etc. Ici `A→N`, `B→O`, …, `M→Z`, `N→A`… Les chiffres et symboles ne sont pas touchés. |
| `cat data.txt \| tr ...` | Variante avec pipe : `tr` ne lit **que** l'entrée standard, il n'accepte pas de nom de fichier, d'où le `cat` ou le `<`. |

## 🔁 Alternatives possibles
- `python3 -c "import codecs; print(codecs.decode(open('data.txt').read(), 'rot13'))"`
- `sed 'y/ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz/NOPQRSTUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm/' data.txt`
- CyberChef, opération « ROT13 ».

## 📚 Ce que j'ai appris
- `tr` pour les substitutions caractère par caractère (aussi utile pour `tr 'a-z' 'A-Z'`, ou `tr -d` pour supprimer des caractères).
- Les chiffrements par substitution simples ne protègent rien.

---
[← Niveau précédent](level10-11.md) · [Sommaire](README.md) · [Niveau suivant →](level12-13.md)
