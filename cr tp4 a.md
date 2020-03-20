# CR TP 4 - Utilisateurs, groupes et permissions


## Exercice 1. Gestion des utilisateurs et des groupes


### 1. Commencez par créer deux groupes groupe1 et groupe2 :
> On utilise la commande ````sudo addgroup groupe1````.
>
### 2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell :
> On utilise la commande ````sudo useradd u1 -m --shell /bin/bash````.
> 
### 3. Ajoutez les utilisateurs dans les groupes créés :
> On utilise la commande ````sudo gpasswd -a u1 groupe1````.
> 
### 4. Donnez deux moyens d’afficher les membres de groupe2 :
>  On utilise la commande ````cut -d: -f1-4 /etc/group```` ou ````cut -d: -f1-4 /etc/gshadow````.
>  
### 5. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4 : 
> On utilise la commande ````sudo chgrp ''groupe1'' ''/home/u1''````.
> 
###  6. Remplacez le groupe primaire des utilisateurs :
> On utilise la commande ````sudo usermod u1 -g groupe1````.

### 7. Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé :  
> On utilise la commande ````sudo mkdir groupe1```` dans le répertoire home puis ````sudo chmod g+rwx groupe1```` pour mettre en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
> 
### 8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ? 
> On utilise la commande ````sudo chmod +t dossier```` pour activer le sticky bit et permettre au propriétaire du fichier de renommer ou supprimer ce fichier.

### 9. Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
> On ne peut pas se connecter en tant que u1 grâce à ````su u1```` car on n'a pas défini de mot de passe à l'utilisateur.

### 10. Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte :
> Il faut donc définir un mot de passe avec la commande ````sudo passwd u1```` et utiliser la commande  ````su u1```` pour se connecter.

### 11. Quels sont l’uid et le gid de u1 ? 
> On utilise la commande ````id u1```` et on trouve 1001 pour les deux.

### 12. Quel utilisateur a pour uid 1003 ?
> En tapant ````id 1003```` et on trouve que c'est l'utilisateur u3.

### 13. Quel est l’id du groupe groupe1 ?
> On utilise la commande ````getent group groupe1```` et on trouve 1001.

### 14. Quel groupe a pour guid 1002 ?
> On utilise la commande ````getent group 1002```` et on trouve groupe2.

### 15. Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez : 
> On utilise la commande ```sudo gpasswd -d u3 groupe2```
> On a enlever u3 du groupe2 cependant lorque qu'on tape ```id u3``` on se rend compte que le groupe primaire de u3 est toujours groupe2. On peut donc en conclure que l'on peut pas retirez un utilisateur de son groupe primaire (sans le remettre dans un autre).

### 16. Modifiez le compte de u4 :
> ```sudo chage -m 5 -M 90 -W 76 -E $((`date '+%s' -d 2020/06/1` /86400)) -I 30 u4```

### 17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
> C'est BASH.
>
### 18. à quoi correspond l’utilisateur nobody ?
> L'utilisateur nobody est un "non-privileged-user" ainsi il n'a aucun droit. Mais il joue un rôle dans le lancement de certains services, comme deamon. Nobody limite les dégats causés par un utilisateur mal-intentionné voulant prendre le contrôle d'utilisateurs sur un serveur. 

### 19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?
> La commande ``sudo`` conserve notre mot de passe en mémoire 15 minutes. 
> C'est la commande  ``sudo -k`` qui permet de forcer ``sudo`` à oublier votre mot de passe.

## Exercice 2. Gestion des permissions

### 1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?
> Après avoir créé le dossier et le fichier demandé, on se rend compte que **test** et **fichier** n'ont pas les mêmes droits. Le dossier est **rwx** pour l'utilisateur et  **r_x** pour le groupe et les autres utilisateurs. Alors que le fichier est **rw_** pour l'utilisateur er **r__** pour le groupe et les autres utilisateurs.
> 
### 2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?
> On arrive a modifier normalement le fichier, donc on peut conclure que le root passe au dessus des droits.

### 3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
> Pour nous redonner les droit d'écriture et d'exécution sur le fichier on fait ``sudo chmod 333 fichier``.
> Ensuite on tape la commande ``echo Hello`` et on se redonne les droit de lecture sur le fichier en tapant ``sudo chmod 777 fichier``. On remarque que le contenu initial du ficher a été remplacé par "echo Hello". 

### 4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.
>  En ayant uniquement les droits d'écriture et d'exécution, on exécute le fichier avec la commande ``./fichier``. On remarque que nous n'avons pas la permission d'exécution du fichier. Par contre, grâce à la commande ``sudo`` on affiche "Hello". 
>  On en déduit donc qu'il faut avoir les droits de lecture pour pouvoir exécuter un programme.

### 5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test :
> On remarque que si on s'enlève les droit de lecture on ne peut pas afficher le contenu du répertoire ni exécuter quelconque fichier du répertoire.

### 6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez vous déduire de toutes ces manipulations ?
> Pour nous enlever les droit d'écriture sur le fichier **nouveau** et le dossier **test** on fait ``sudo chmod 555 nouveau`` et  ``sudo chmod 555 test``.
> Quand on essaye de modifier le fichier **nouveau** on remarque que **nano** nous affiche en rouge en bas de l'écran **File 'nouveau' is unwritable** et qu'il ne nous permet pas d'enregistrer les modifications.
> Pour nous redonner les droit d'écriture et d'exécution sur **test** fichier on fait ``sudo chmod 777 test``.
> Malgré cela on ne peut toujours pas modifier le fichier **nouveau** ou le supprimer.

### 7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
> Pour nous enlever les droit d'exécution sur le dossier **test** on fait ``sudo chmod 666 test``.
> Après avoir fait cela on ne peut ni créer, ni supprimer, ou modifier un fichier du répertoire **test** et encore moins s'y déplacer. Par contre on arrive quand même à en lister le contenu.
> Le droit d'exécution est donc le plus fort des 3 (pour les répertoire) car on peu uniquement lister le contenu du répertoire, alors que par exemple sans le droit de d'écriture on peut tout faire pour peu que l'on sache le contenu du répertoire.

### 8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?
> Pour nous enlever les droit d'exécution sur le dossier courant **test** on fait ``sudo chmod 666 /home/test``.
> Ici on n'arrive rien à faire, même pas lister le contenu du répertoire courant. On ne peut donc exécuter aucune commande.
> Mais on peut utiliser ``cd..``, c'est surement une sécurité pour ne pas se retrouver bloqué dans le répertoire.

### 9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
> Pour donner à une autre personne de notre groupe les droits suffisants pour y accéder en lecture, mais pas en écriture on tape la commande ``sudo chmod 747 fichier`` depuis le répertoire **test**.

### 10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
> Un *umask* très restrictif qui interdit à quiconque à part nous l’accès en lecture ou en écriture, ainsi que la traversée de nos répertoires est ``umask 077``. En effet grâce à ce masque seul l'utilisateur a tous les droits, les membres du groupe ou les autres n'ont aucuns droits.
> On retrouve cela lors du test.

### 11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
> Un *umask* très permissif qui autorise tout le monde à lire nos fichiers et traverser nos répertoires, mais n’autorise que nous à écrire, est ``umask 022``. En effet grâce à ce masque seul l'utilisateur a le droit d'écrire, les membres du groupe ou les autres ne l'ont pas.
> On retrouve cela lors du test.

### 12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
> Un *umask* équilibré qui nous autorise un accès complet et autorise un accès en lecture aux membres de notre groupe est  ``umask 037``. En effet grâce à ce masque seul l'utilisateur a tous les droits, les membres du groupe ont le droit de lecture et les autres n'ont aucuns droits.
> On retrouve cela lors du test.

### 13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :
>  >  -``chmod u=rx,g=wx,o=r fic`` donne ``chmod 534 fic``
>  >  -``chmod uo+w,g-rx fic`` donne ``chmod 602 fic`` en sachant que les droits initiaux de fic sont r--r-x---
>   > -``chmod 653 fic`` donne ``chmod u-x, g+r, o+w fic`` en sachant que les droits initiaux de fic sont 711
>  > -``chmod u+x,g=w,o-r fic`` donne ``chmod 520 fic`` en sachant que les droits initiaux de fic sont r--r-x---

### 14. Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?
> En tapant la commande ``ll /bin/passwd`` on trouve que nos droits sont les suivants ``-rwsr-xr-x``. Le s signifie que **passwd** est exécuté avec les droits de son propriétaire, donc cela veut dire que tout utilisateur exécutant **passwd**, l'exécute comme s'il était **root**. cd 
Les droits du fichier **/etc/passwd** sont ``-rw-r--r--``. Or ce fichier appartient à **root**, et donc en temps normal seul **root** pourrait écrire dedans. D'où l'intérêt de ce **s** qui permet à tous les utilisateurs de modifier leur mot de passe.
