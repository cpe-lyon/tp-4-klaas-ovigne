OVIGNE Adrien *(INFRA)* & KLAAS Guillaume *(INFO)*

# TP4  Utilisateurs, groupes et permissions

## Exercice 1.

1. *Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?*

Bash trouve les commandes dans les dossiers affichés par la commande **printenv PATH**.

2. *Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans
votre répertoire personnel ?*

Il s'agit de la variable d'environnement **HOME**.

3. *Explicitez le rôle des variables **LANG, PWD, OLDPWD, SHELL et _**.*

**LANG**: langue utilisée pr communiquer avec l'utilisateur.

**PWD**: affiche le repertoire courant 

**OLDPWD**: affiche l'ancien repertoire courant

**SHELL**:  montre le chemin de l'interpreteur shell utilisé par defaut

**\_**:  montre le chemin vers la commande qui est executée

4. *Créez une variable locale **MY_VAR** (le contenu n’a pas d’importance). Vérifiez que la variable existe.* 

On entre les commandes suivantes: **MY_VAR="test"** puis **echo $MY_VAR**

5. *Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin*
*de cette question, tapez la commande exit pour revenir dans votre session initiale.*

**bash** nous place dans un nouveau shell, la variable locale MY\_VAR n'y existe pas. un **echo $MY\_VAR** n'affiche donc rien apres avoir fait **exit** on revient dans le shell où MY\_VAR existe.

6. *Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.*

**export MY_VAR="test_env"** permet de transformer MY_VAR en variable d'environnement, elle reste donc accessible même après la manipulation de la question précédente.

7. *Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace.*
*Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.*

**export NOMS="KLAAS OVIGNE" ; printenv NOMS**

8. *Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2*
*sont vos deux noms) en utilisant la variable NOMS.*

**echo Bonjour à vous deux, $NOMS!**
 
9. *Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande*
*unset ?*

**unset** supprime la variable.
 
10. *Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre*
*dossier personnel d’après bash)*

**echo \$HOME = $HOME** le "\" permet d'afficher "HOME" au lieu du contenu de la variable d'environnement.
11.

12.

13.

14.

15.

16.

17.

18.

19.

