# Bandit, niveau 16 → 17

## Le but
Un seul port entre 31000 et 32000 renvoie les identifiants du niveau suivant. Il faut d'abord trouver quels ports sont ouverts, lesquels parlent TLS, puis lequel ne se contente pas de renvoyer ce qu'on lui envoie (*echo*).

## Comment j'ai fait
1. Découverte : scanner la plage de ports pour lister ceux qui écoutent.
2. Identification : déterminer pour chaque port ouvert le service et s'il utilise SSL/TLS.
3. Élimination : les services `echo` renvoient simplement le texte reçu ; il reste un seul port SSL qui n'est pas un echo.
4. Exploitation : envoyer le mot de passe actuel à ce port en TLS (comme au niveau 15).
5. La réponse n'est pas un mot de passe mais une clé privée RSA : je la sauvegarde dans un fichier local, `chmod 600`, puis `ssh -i` pour me connecter au niveau suivant (comme au niveau 13).

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `nmap -p 31000-32000 localhost` | Scanner de ports : teste chaque port de la plage et indique s'il est ouvert. |
| `-sV` | *Service/Version detection* : envoie des sondes pour identifier le service (echo, ssl/echo, ssl/unknown…). Plus lent mais beaucoup plus informatif. |
| `cat /etc/bandit_pass/bandit16 \| openssl s_client -quiet -connect localhost:<port>` | Envoie le mot de passe actuel au port candidat en TLS. |
| `chmod 600 cle.private` + `ssh -i cle.private ...` | Utiliser la clé reçue (voir niveau 13). |

## Autres façons de faire
- `ss -tlnp` ou `netstat -tlnp` : comme on est sur la machine, on peut lister directement les sockets en écoute, sans scanner.
- `nc -zv localhost 31000-32000` : scan TCP rapide avec netcat (`-z` = juste tester, sans envoyer de données).
- Boucle bash : `for p in $(seq 31000 32000); do (echo > /dev/tcp/localhost/$p) 2>/dev/null && echo "$p ouvert"; done`.
- Tester le TLS port par port : `openssl s_client -connect localhost:<port> </dev/null` (si un certificat s'affiche, c'est du TLS).

## À retenir
- La méthodologie découverte → identification → élimination → exploitation, la même qu'en pentest réel.
- `nmap` et son option `-sV`.

---
[niveau précédent](level15-16.md) · [sommaire](README.md) · [niveau suivant](level17-18.md)
