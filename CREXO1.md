# Compte Rendu TP4 Administration système

## Exercice 1. Gestion des utilisateurs et des groupes

1 `sudo addgroup nom_groupe` permet d'ajouter un groupe.

2 `sudo useradd uX -m --shell /bin/bash` permet d'ajouter un utiliseur et définir son shell.

3 `sudo gpasswd -a uX groupeX` permet d'ajouter un utilisateur `uX` à un groupe `groupeX`.

4 `getent group groupe2` & `grep groupe2 /etc/group | cut -d: -f4`.

5 `chgrp groupeX /home/uX` permet de changer le groupe de `/home/uX` à `groupeX`. Exemple: `chrgrp groupe2 /home/u2`.

6 `usermod -g groupeX uX` permet de changer le groupe primaire d'un utilisateur `uX` vers le groupe `groupeX`.

7 On commence par créer le répertoire `mkdir /home/groupeX`
 Puis on fixe le propriétaire `chgrp groupeX /home/groupeX`
 Enfin on donne des droits, ici lecture & ecriture `chmod g+w /home/groupeX`
 
8

9 On ne peux pas se connecter en tant qu'utilisateur u1 son compte est inactif, car il n'a pas de mot de passe.

10 Pour activer le compte u1 on lui attribue un mot de passe:
`passwd u1'
pour vérifier que le compte est désormais actif on tente de se connecter:
'su u1 -`

11 On utilise la commande `id u1`. On obtient un uid = 1001 et un gid = 1003

12 Il s'agit de `u3`.

13 L'id du groupe1 est 1001 celui du groupe2 est 1002 on l'obtient à la création du groupe ou dans le fichier `/etc/group`.

14 Le groupe2est le possesseur de l'id 1002. 

15 Pour enlever un groupe à u3 on utilise la commande `gpasswd -d groupe_a_enlever` en l'occurence le groupe2. 
Après avoir enlevé u3 au groupe2 on remarque que u3 est toujours considéré comme étant dans le groupe2 car u3 n'a qu'un groupe,
au prochain démarrage un groupe u3 sera créé et contiendra uniquement u3.

16 On utilise la commande `chage` pour modifier les paramètres du mot de passe de u4, on rappelle qu'un mot de passe périmé
revient à désactiver un compte

Pour la date `chage -E YYYY-MM-DD uX` ici YYYY = 2020 MM = 06, DD = 01 et uX = u4.
Pour la durée maximale avant changement du mot de passe `chage -M 90 u4`.
Pour la durée minimale avant changement du mot de passe `chage -m 5 u4`.
Pour le Warning: `chage -W 14 u4`.
Pour le blocage: `chage -I 30 u4`.

17 `sudo -u root printenv SHELL` et on obtient `/bin/bash`.

18 `nobody` est un pseudo-utilisateur qui n'a que les droits minimaux. Il est/était utilisé 
pour lancer des services sensibles afin de limiter les risques en cas d'attaque mais cela perd son sens
lorsque plusieurs processus sont lancés en même temps par "nobody".

19 Le temps par défaut est 5 minutes (dépend des sources / distributions), il peut être modifié grâce à la commande `visudo`
qui nous donne accès à un fichier où il faudra ajouter/changer la variable timestamp_timeout.
