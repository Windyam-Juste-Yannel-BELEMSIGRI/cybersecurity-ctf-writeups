# Bandit, niveau 22 → 23

## Le but
Même principe : un programme tourne avec cron, il faut lire sa configuration dans `/etc/cron.d/`. L'énoncé conseille de regarder les scripts écrits par d'autres et de les exécuter soi-même pour voir ce qu'ils affichent.

## Comment j'ai fait
1. `cat /etc/cron.d/cronjob_bandit23` : la tâche lance `/usr/bin/cronjob_bandit23.sh` en tant que `bandit23`, toutes les minutes.
2. Je lis le script :
   ```bash
   myname=$(whoami)
   mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
   echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
   cat /etc/bandit_pass/$myname > /tmp/$mytarget
   ```
   Il copie le mot de passe de l'utilisateur qui le lance dans `/tmp/<hash>`, où le hash dépend du nom de l'utilisateur.
3. Le piège : si je lance le script moi-même, `whoami` vaut `bandit22`, donc il copie mon mot de passe, pas celui de `bandit23`. Ça sert quand même : ça me montre le message et le format du nom de fichier.
4. Quand cron le lance, `whoami` vaut `bandit23`. Donc je recalcule le nom du fichier à la main, avec `bandit23` à la place de `$myname` :
   ```bash
   echo I am user bandit23 | md5sum | cut -d ' ' -f 1
   ```
5. Je fais `cat /tmp/<le hash obtenu>` : c'est le fichier que cron remplit en tant que `bandit23`.

## Les commandes et ce qu'elles font vraiment
| Commande | Ce qu'elle fait |
|---|---|
| `whoami` | Affiche le nom de l'utilisateur effectif qui exécute la commande. Dans un script lancé par cron en tant que `bandit23`, il renvoie `bandit23`. |
| `$( ... )` | Substitution de commande : exécute la commande et met son résultat à la place, ici dans une variable. |
| `echo I am user bandit23` | Produit le texte à hacher. Attention, `echo` ajoute un retour à la ligne à la fin, et il fait partie de ce qui est haché. |
| `md5sum` | Calcule l'empreinte MD5 de ce qu'il reçoit et affiche `<hash>  -` (le `-` = entrée standard). |
| `cut -d ' ' -f 1` | Découpe sur les espaces et garde le 1er champ : le hash seul, sans le `-`. |
| `/usr/bin/cronjob_bandit23.sh` | Lancer le script moi-même pour voir son comportement, comme le suggère l'énoncé. |

## Autres façons de faire
- `echo I am user bandit23 | md5sum | awk '{print $1}'` : même chose avec `awk`.
- Copier le script, remplacer `$(whoami)` par `bandit23` et le lancer : il affiche directement le chemin du fichier (il échouera sur le `cat`, faute de droits, mais le message suffit).
- Piège : `echo -n I am user bandit23 | md5sum` donne un autre hash, parce que `-n` enlève le retour à la ligne. Il faut reproduire exactement ce que fait le script.

## À retenir
- Un hash n'est pas un secret : si on connaît l'entrée (ici le texte et le nom d'utilisateur), on peut recalculer le résultat. Un nom de fichier « imprévisible » calculé à partir d'infos publiques ne protège rien.
- Le contexte d'exécution compte : le même script ne fait pas la même chose selon qui le lance.
- Lire un script avant de l'exécuter, et l'exécuter dans un cadre sûr pour comprendre ce qu'il fait : c'est une bonne habitude d'analyste.

---
[niveau précédent](level21-22.md) · [sommaire](README.md) · [niveau suivant](level23-24.md)
