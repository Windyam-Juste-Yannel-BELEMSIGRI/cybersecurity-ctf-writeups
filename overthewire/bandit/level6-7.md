# Bandit, niveau 6 → 7

## Le but
Trouver un fichier n'importe où sur le serveur qui appartient à l'utilisateur `bandit7`, au groupe `bandit6` et fait 33 octets.

## Comment j'ai fait
1. Cette fois la recherche part de la racine `/` : tout le système.
2. Je reprends `find` avec les critères propriétaire, groupe et taille.
3. Problème : la sortie est noyée sous des centaines de messages `Permission denied` (dossiers que mon utilisateur ne peut pas lire). Ces messages partent sur la sortie d'erreur, il suffit donc de la rediriger.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `find / -type f -user bandit7 -group bandit6 -size 33c` | `-user` / `-group` filtrent sur le propriétaire et le groupe du fichier. |
| `2>/dev/null` | Chaque processus a 3 flux : 0 = entrée (stdin), 1 = sortie (stdout), 2 = erreurs (stderr). `2>` redirige les erreurs vers `/dev/null`, un « trou noir » qui jette tout ce qu'on y écrit. Il ne reste que les vrais résultats. |

## Autres façons de faire
- `find / ... 2>&1 | grep -v "Permission denied"` : fusionne stderr dans stdout, puis retire les lignes d'erreur.
- `find / -readable ...` : ne retient que les fichiers que je peux réellement lire.
- `locate` (si la base est à jour) ne permet pas ces filtres : `find` reste le bon outil ici.

## À retenir
- Les 3 flux standards et les redirections (`>`, `2>`, `2>&1`, `/dev/null`).
- Propriétaire et groupe d'un fichier, notions de base des permissions Linux.

---
[niveau précédent](level5-6.md) · [sommaire](README.md) · [niveau suivant](level7-8.md)
