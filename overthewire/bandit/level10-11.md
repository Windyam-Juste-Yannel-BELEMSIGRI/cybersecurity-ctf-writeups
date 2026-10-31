# Bandit — Niveau 10 → 11

## 🎯 Objectif
`data.txt` contient des données encodées en **Base64** : les décoder.

## 🧭 Cheminement
1. Un `cat data.txt` montre un texte composé de lettres, chiffres, `+`, `/` et terminé souvent par `=` : signature typique du Base64.
2. Il suffit de le décoder.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `base64 -d data.txt` | `base64` encode par défaut ; `-d` (*decode*) fait l'opération inverse. |

## 🔁 Alternatives possibles
- `openssl base64 -d -in data.txt`
- `python3 -c "import base64,sys; print(base64.b64decode(open('data.txt').read()).decode())"`
- CyberChef (outil web, opération « From Base64 »).

## 📚 Ce que j'ai appris
- **Base64 n'est pas du chiffrement** : c'est un *encodage* réversible sans clé, qui sert à transporter des données binaires sous forme de texte (emails, URLs, JSON…). Si on trouve un « secret » en Base64, il est en clair.

---
[← Niveau précédent](level9-10.md) · [Sommaire](README.md) · [Niveau suivant →](level11-12.md)
