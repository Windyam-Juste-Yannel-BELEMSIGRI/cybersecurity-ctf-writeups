# Bandit, niveau 0 → 1

## Le but
Se connecter au serveur du jeu en SSH avec les identifiants fournis sur la page du niveau, puis lire le fichier qui contient le mot de passe du niveau suivant.

## Comment j'ai fait
1. La page du niveau donne trois infos : l'hôte (`bandit.labs.overthewire.org`), le port 2220 et l'utilisateur `bandit0`.
2. SSH écoute par défaut sur le port 22 : comme ici le port est différent, il faut le préciser explicitement.
3. Une fois connecté, je regarde ce qu'il y a dans mon répertoire personnel, et j'affiche le fichier `readme`.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `ssh bandit0@bandit.labs.overthewire.org -p 2220` | Ouvre une session shell chiffrée sur une machine distante. `user@hôte` indique avec quel compte se connecter, `-p` change le port TCP de destination. |
| `ls` | Liste le contenu d'un répertoire (ici le *home*, `~`). |
| `cat readme` | *Concatenate* : lit un ou plusieurs fichiers et envoie leur contenu sur la sortie standard (l'écran). |
| `exit` / `Ctrl+D` | Ferme la session SSH proprement. |

## Autres façons de faire
- `ssh -p 2220 bandit0@bandit.labs.overthewire.org` : l'ordre des options n'a pas d'importance.
- Créer une entrée dans `~/.ssh/config` pour ne plus retaper l'hôte et le port :
  ```
  Host bandit
      HostName bandit.labs.overthewire.org
      Port 2220
  ```
  puis `ssh bandit0@bandit`.
- Sous Windows : le client OpenSSH est intégré à PowerShell, sinon PuTTY.
- Pour lire le fichier : `less readme`, `more readme`, `head readme`.

## À retenir
- Le fonctionnement de base de SSH (client, serveur, port).
- Qu'un service peut écouter sur un port non standard et qu'il faut alors le préciser.

---
[sommaire](README.md) · [niveau suivant](level1-2.md)
