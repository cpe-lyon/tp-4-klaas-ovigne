OVIGNE Adrien *(INFRA)* & KLAAS Guillaume *(INFO)*

# TP4  Utilisateurs, groupes et permissions

## Exercice 1.

1. *Commencez par créer deux groupes groupe1 et groupe2**

Bash trouve les commandes dans les dossiers affichés par la commande **printenv PATH**.

2. *Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell*

Il s'agit de la variable d'environnement **HOME**.

3. *Ajoutez les utilisateurs dans les groupes créés :
- u1, u2, u4 dans groupe1
- u2, u3, u4 dans groupe2*

**LANG**: langue utilisée pr communiquer avec l'utilisateur.

**PWD**: affiche le repertoire courant 

**OLDPWD**: affiche l'ancien repertoire courant

**SHELL**:  montre le chemin de l'interpreteur shell utilisé par defaut

**\_**:  montre le chemin vers la commande qui est executée

4. *Donnez deux moyens d’afficher les membres de groupe2*

On entre les commandes suivantes: **MY_VAR="test"** puis **echo $MY_VAR**

5. *. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire
de /home/u3 et /home/u4*

**bash** nous place dans un nouveau shell, la variable locale MY\_VAR n'y existe pas. un **echo $MY\_VAR** n'affiche donc rien apres avoir fait **exit** on revient dans le shell où MY\_VAR existe.

6. *Remplacez le groupe primaire des utilisateurs :
- groupe1 pour u1 et u2
- groupe2 pour u3 et u4*

**export MY_VAR="test_env"** permet de transformer MY_VAR en variable d'environnement, elle reste donc accessible même après la manipulation de la question précédente.

7. *Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.*

**export NOMS="KLAAS OVIGNE" ; printenv NOMS**

8.*Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?*
**echo Bonjour à vous deux, $NOMS!**
 
9. *Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?*

**unset** supprime la variable.
 
10. *Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte.*

**echo \$HOME = $HOME** le "\" permet d'afficher "HOME" au lieu du contenu de la variable d'environnement.
11. *Quels sont l’uid et le gid de u1 ?*

12. *Quel utilisateur a pour uid 1003 ?*

13. *Quel est l’id du groupe groupe1 ?*

14. *Quel groupe a pour guid 1002 ? *

15. *Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.*

16. *Modifiez le compte de u4 de sorte que :
— il expire au 1er juin 2020
— il faut changer de mot de passe avant 90 jours
— il faut attendre 5 jours pour modifier un mot de passe
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
— le compte sera bloqué 30 jours après expiration du mot de passe*

17. *Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?*

18. *à quoi correspond l’utilisateur nobody ?*

19. *Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
Quelle commande permet de forcer sudo à oublier votre mot de passe ?*

