# Bandit — Niveau 15 → 16

## 🎯 Objectif
Même principe qu'au niveau précédent, mais le service du **port 30001** attend une connexion **chiffrée SSL/TLS**.

## 🧭 Cheminement
1. `nc` envoie du texte en clair : le serveur TLS ne le comprendrait pas (il attend d'abord une négociation TLS, le *handshake*).
2. Il faut un client capable de faire ce handshake : `openssl s_client`.
3. Une fois connecté, j'envoie le mot de passe actuel.
4. Piège signalé par l'énoncé : en mode interactif, `s_client` interprète certaines lignes qui commencent par une lettre spéciale (`Q`, `R`, `K`…) comme des **commandes** (quitter, renégocier, mettre à jour la clé). D'où les messages `DONE`, `RENEGOTIATING`, `KEYUPDATE`. La solution est de désactiver ce mode.

## 🛠️ Commandes utilisées et leur vrai rôle
| Commande | Rôle réel |
|---|---|
| `openssl s_client -connect localhost:30001` | Client TLS de test : établit la connexion chiffrée, affiche le certificat du serveur, puis relaie le clavier ↔ serveur. |
| `-quiet` | Masque les infos de session/certificat **et** active `-ign_eof` : plus d'interprétation des « commandes connectées ». |
| `-ign_eof` | Ne ferme pas la connexion à la fin de l'entrée et désactive les commandes `Q`/`R`/`K`. |
| `cat /etc/bandit_pass/bandit15 \| openssl s_client -quiet -connect localhost:30001` | Envoie le mot de passe via un pipe : pas de faute de copier-coller, et pas de mot de passe tapé en clair dans l'historique. |

## 🔁 Alternatives possibles
- `ncat --ssl localhost 30001` (netcat de la suite nmap, avec support TLS).
- `socat - OPENSSL:localhost:30001,verify=0` (`verify=0` car le certificat est auto-signé).

## 📚 Ce que j'ai appris
- La différence entre une connexion TCP en clair et une connexion TLS.
- Lire la section **CONNECTED COMMANDS** de `man s_client` : l'énoncé donnait l'indice, d'où l'intérêt de lire les manpages.

---
[← Niveau précédent](level14-15.md) · [Sommaire](README.md) · [Niveau suivant →](level16-17.md)
