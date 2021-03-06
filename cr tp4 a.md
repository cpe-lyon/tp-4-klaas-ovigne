OVIGNE Adrien *(INFRA)* & KLAAS Guillaume *(INFO)*

# TP4  Utilisateurs, groupes et permissions

## Exercice 1. Gestion des utilisateurs et des groupes

1. *Commencez par créer deux groupes groupe1 et groupe2*

`sudo addgroup nom_groupe` permet d'ajouter un groupe.

&nbsp;

2. *Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell*

`sudo useradd uX -m --shell /bin/bash` permet d'ajouter un utiliseur et définir son shell.

&nbsp;

3. *Ajoutez les utilisateurs dans les groupes créés :*
- *u1, u2, u4 dans groupe1*
- *u2, u3, u4 dans groupe2*

`sudo gpasswd -a uX groupeX` permet d'ajouter un utilisateur `uX` à un groupe `groupeX`.

&nbsp;


4. *Donnez deux moyens d’afficher les membres de groupe2*

`getent group groupe2` & `grep groupe2 /etc/group | cut -d: -f4`.

&nbsp;

5. *Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4*

`chgrp groupeX /home/uX` permet de changer le groupe de `/home/uX` à `groupeX`. Exemple: `chrgrp groupe2 /home/u2`.

&nbsp;

6. *Remplacez le groupe primaire des utilisateurs :*
- *groupe1 pour u1 et u2*
- *groupe2 pour u3 et u4*

`usermod -g groupeX uX` permet de changer le groupe primaire d'un utilisateur `uX` vers le groupe `groupeX`.

&nbsp;

7. *Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.*

On commence par créer le répertoire `mkdir /home/groupeX`

Puis on fixe le propriétaire `chgrp groupeX /home/groupeX`

Enfin on donne des droits, ici lecture & ecriture `chmod g+w /home/groupeX`

&nbsp;

8. *Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?*
 
Il faut activer le stick bit : `chmod +t fichier`
 
 &nbsp;

9. *Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?*

On ne peux pas se connecter en tant qu'utilisateur u1 son compte est inactif, car il n'a pas de mot de passe.
 
 &nbsp;

10. *Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.*

Pour activer le compte u1 on lui attribue un mot de passe: `passwd u1`

pour vérifier que le compte est désormais actif on tente de se connecter: 'su u1 -`

&nbsp;

11. *Quels sont l’uid et le gid de u1 ?*

On utilise la commande `id u1`. On obtient un uid = 1001 et un gid = 1003

&nbsp;

12. *Quel utilisateur a pour uid 1003 ?*

Il s'agit de `u3`.

&nbsp;


13. *Quel est l’id du groupe groupe1 ?*

L'id du groupe1 est 1001 celui du groupe2 est 1002 on l'obtient à la création du groupe ou dans le fichier `/etc/group`.

&nbsp;


14. *Quel groupe a pour guid 1002 ?*

Le groupe2est le possesseur de l'id 1002.

&nbsp;


15. *Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.*

Pour enlever un groupe à u3 on utilise la commande `gpasswd -d groupe_a_enlever` en l'occurence le groupe2.  

Après avoir enlevé u3 au groupe2 on remarque que u3 est toujours considéré comme étant dans le groupe2 car u3 n'a qu'un groupe, au prochain démarrage un groupe u3 sera créé et contiendra uniquement u3.

&nbsp;


16. *Modifiez le compte de u4 de sorte que :*
- *il expire au 1er juin 2020*
- *il faut changer de mot de passe avant 90 jours*
- *il faut attendre 5 jours pour modifier un mot de passe*
- *l’utilisateur est averti 14 jours avant l’expiration de son mot de passe*
- *le compte sera bloqué 30 jours après expiration du mot de passe*

Pour la date `chage -E YYYY-MM-DD uX` ici YYYY = 2020 MM = 06, DD = 01 et uX = u4.

Pour la durée maximale avant changement du mot de passe `chage -M 90 u4`.

Pour la durée minimale avant changement du mot de passe `chage -m 5 u4`.

Pour le Warning: `chage -W 14 u4`.

Pour le blocage: `chage -I 30 u4`.

&nbsp;


17. *Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?*

`sudo -u root printenv SHELL` et on obtient `/bin/bash`.

&nbsp;


18. *à quoi correspond l’utilisateur nobody ?*

`nobody` est un pseudo-utilisateur qui n'a que les droits minimaux. Il est/était utilisé  pour lancer des services sensibles afin de limiter les risques en cas d'attaque mais cela perd son sens lorsque plusieurs processus sont lancés en même temps par "nobody".

&nbsp;

19. *Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?*

Le temps par défaut est 5 minutes (dépend des sources / distributions), il peut être modifié grâce à la commande `visudo` qui nous donne accès à un fichier où il faudra ajouter/changer la variable timestamp_timeout.

&nbsp;

## Exercice 2. Gestion des permissions

1. *Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?*

On remarque que le fichier et le dossier n'ont pas les mêmes droits. Les fichier est en rw alors que le dossier est en rwx

&nbsp;

2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

Même sans avoir de droits, en tant que root on peut quand même modifier le fichier. Le root a donc tous les droits, indépendamment des droits sur les fichiers.

&nbsp;

3. *Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?*

On utilise la commande `sudo chmod u+wx fichier` pour récupérer les droits.

Avec la commande `echo "echo Hello" > fichier` en executant le fichier avec `./fichier` il va afficher "Hello".

&nbsp;

4. *Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.*

`cd test/`

`./fichier`

Nous n'avons pas la permission. Cela est du au fait que l'on aie pas le droit de lecture sur ce fichier. En faisant de même en sudo, on reçoit `Hello`.

&nbsp;

5. *Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test*

Pour se retirer les droits de lecture dans le repertoire test : `chmod u-r test`.

Puis en faisant `ls test`, on nous dit que nous n'avons pas la permission (car on n'a pas les droits de lecture).

Pourtant on peut lire le fichier "fichier", car même si nous n'avons plus les droits en lecture sur le repertoire "test", on les a toujours sur le fichier "fichier".

Ensuite pour rétablir ce droit : `chmod u+r test`

&nbsp;

6. *Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?*

Pour enlever le droit en ecriture `sudo chmod u-w nouveau` & `sudo chmod u-w test`

Pour tenter de modifier le fichier nouveau : `echo "test ecriture 1 " > test/nouveau` -> `Permisson denied` (on ne peut pas suprimmer le fichier non plus d'ailleurs).

Rétablir le droit en écriture : `chmod u+w test`

Ensuite `echo "test ecriture 2 " > test/nouveau` -> `Permisson denied`

Tentative de supression : `rm test/nouveau`, le fichier est supprimé.

Lorsque l'on remet les droits en ecriture sur le repertoire, on ne peut pas écrire dans les fichiers qu'il contient (sauf si nous avons l'autorisation sur les fichiers) mais nous pouvons quand même les supprimer.




&nbsp;

7. *Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?*

Supprimer les droits d'éxécution du dossier bloque toute action sur le dossier. En utilisant `ls` nous pouvons quand même obtenir les noms des fichiers/dossiers à l'intérieur mais c'est tout.
&nbsp;

8. *Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?*

On peut conclure que nos permissions dépende des permissions du repertoire dans lequel on se trouve. La commande **cd ..** marche car on a les permissions nécéssaires dans le repertoire parent.

&nbsp;

9. *Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.*

Rétablir le droit d'execution : `chmod u+x test`

Ajouter les droits en lecture au groupe : `chmod g+r test/fichier`

Enlever le droit en ecriture : `chmod g-w test/fichier`

&nbsp;

10. *Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.*

on souhaite donc des permissions du type : 111-000-000.

On utilise donc `umask 077`

&nbsp;

11. *Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.*

De la même manière, ici on souhaite des permissions comme ceci : 111-101-101
On utilise donc `umask 022`

&nbsp;

12. *Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.*

De la même manière, ici on souhaite des permissions comme ceci : 111-100-000
On utilise donc `umask 037`

&nbsp;

13. *Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous*
*pourrez vous aider de la commande stat pour valider vos réponses) :*
- *chmod u=rx,g=wx,o=r fic*
- *chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---*
- *chmod 653 fic en sachant que les droits initiaux de fic sont 711*
- *chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---*

chmod u=rx,g=wx,o=r fic -> chmod 534 fic

chmod uo+w,g-rx fic     -> chmod 604 fic

chmod 653 fic           -> chmod u-x,g-r,o-w fic

chmod u+x,g=w,o-r fic   -> chmod 520 fic


&nbsp;

14. *Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?*

`stat -c %A /etc/passwd` -> `-rw-r--r--`

Sur ce programme, les utilisateurs *group* et *other* n'ont que le droit en lecture, et non en écriture. Effectivement il faut être *root* pour pouvoir gérer les utilisateurs.
