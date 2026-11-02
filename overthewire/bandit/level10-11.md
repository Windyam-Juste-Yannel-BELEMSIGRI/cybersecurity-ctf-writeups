# Bandit, niveau 10 → 11

## Le but
`data.txt` contient des données encodées en **Base64** : les décoder.

## Comment j'ai fait
1. Un `cat data.txt` montre un texte composé de lettres, chiffres, `+`, `/` et terminé souvent par `=` : signature typique du Base64.
2. Il suffit de le décoder.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `base64 -d data.txt` | `base64` encode par défaut ; `-d` (*decode*) fait l'opération inverse. |

## Autres façons de faire
- `openssl base64 -d -in data.txt`
- `python3 -c "import base64,sys; print(base64.b64decode(open('data.txt').read()).decode())"`
- CyberChef (outil web, opération « From Base64 »).

## À retenir
- **Base64 n'est pas du chiffrement** : c'est un *encodage* réversible sans clé, qui sert à transporter des données binaires sous forme de texte (emails, URLs, JSON…). Si on trouve un « secret » en Base64, il est en clair.

---
[niveau précédent](level9-10.md) · [sommaire](README.md) · [niveau suivant](level11-12.md)
