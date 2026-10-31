# Bandit — Niveau 14 → 15

## 🎯 Objectif
Envoyer le mot de passe du niveau actuel au **port 30000** de `localhost` pour recevoir le suivant.

## 🧭 Cheminement
1. Un service écoute en TCP sur le port 30000 de la machine elle-même (`localhost`).
2. Il me faut un outil capable d'ouvrir une connexion TCP brute et d'y envoyer du texte : `nc` (netcat).
3. Le mot de passe actuel se trouve dans le dossier standard des mots de passe ; je peux le lire puisque je suis `bandit14`. Je l'envoie directement par un pipe pour éviter de le retaper.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `cat /etc/bandit_pass/bandit14` | Lit le mot de passe du niveau actuel (lisible uniquement par son propriétaire). |
| `nc localhost 30000` | *Netcat*, le « couteau suisse » du réseau : ouvre une connexion TCP vers l'hôte et le port indiqués. Tout ce qui arrive sur son entrée standard est envoyé ; tout ce que le serveur répond s'affiche. |
| `cat ... \| nc localhost 30000` | Le pipe envoie le contenu du fichier dans la connexion. |

## 🔁 Alternatives possibles
- `nc localhost 30000 < /etc/bandit_pass/bandit14` : redirection au lieu du pipe.
- `telnet localhost 30000` puis coller le mot de passe.
- Sans aucun outil, en bash pur : `exec 3<>/dev/tcp/localhost/30000; cat /etc/bandit_pass/bandit14 >&3; cat <&3`.
- `socat - TCP:localhost:30000`.

## 📚 Ce que j'ai appris
- Notions de client/serveur TCP, de port et de `localhost` (127.0.0.1).
- `nc` sert à tester un service, transférer des données, ou ouvrir un port en écoute (`nc -l`).

---
[← Niveau précédent](level13-14.md) · [Sommaire](README.md) · [Niveau suivant →](level15-16.md)
