# TP_3_UNIX

### Exercice : paramètres

Pour l'exercice on cherche il faut afficher les paramètres qu'on aura écrit en executant le script bash. Après quelques recherches avec une recherche google on trouve que simplement un `echo $0` affiche le nom du script, du coup je mets ça : 

![image](https://github.com/user-attachments/assets/2853a950-6434-496f-987c-db176dc418dc)

La première ligne s'appelle le "shebang" c'est important pour executer n'importe quelle script, ici du bash. 
Aussi, on a déclaré une variable vide `param = $#` pour stocker les arguments passés au script (cf le cours du tp3). 

### exo 1: 

```
#!/bin/bash
# déclaration d'une variable qui prend en paramètres les arguments passer lors de l éxecution 
parametres=$#
#récupere le nom du fichier dans une variable
nom_script=$(basename "$0")
#affecte le troisieme parametre à la variable, si rien n'est rentré, affecte aucun
troisieme_parametre=${3:-"Aucun"}

#affecte tout les paramètres dans un seul string
liste_parametres="$*"

#affichage
printf "Bonjour, vous avez rentré %d paramètre(s).\n" "$nombre_parametres"
printf "Le nom du script est  $nom_du_script"
printf "Le 3ème paramètre est $troisieme_parametre"
printf "Voici la liste des paramètres : %s\n" "$liste_parametres"
```
### exo 2: 

```
# Vérifie si le nombre de paramètres passés au script est différent de 2
if [ "$#" -ne 2 ]; then
    # Sinon , affiche un message d'erreur

    echo "Erreur : Vous devez rentrer exactement 2 paramètres."
    # Quitte le script avec un code d'erreur 1 (indiquant une erreur)
    exit 1
fi

# Concatène les deux premiers paramètres ($1 et $2) et les stocke dans la variable CONCAT
CONCAT="$1$2"

# Affiche le résultat de la concaténation
echo "La concaténation des deux mots est : $CONCAT"
```
### exo 3:

```
#!/bin/bash

# Vérifie si le fichier a été passé en paramètre
if [ "$#" -ne 1 ]; then
    echo "Erreur : Vous devez fournir un chemin de fichier en paramètre."
    exit 1
fi

# Stocke le fichier passé en paramètre
FICHIER="$1"

# Vérifie si le fichier existe
if [ ! -e "$FICHIER" ]; then
    echo "Le fichier $FICHIER n'existe pas."
                                                                                           
    exit 1
fi

# Stocke le fichier passé en paramètre
FICHIER="$1"

# Vérifie si le fichier existe
if [ ! -e "$FICHIER" ]; then
    echo "Le fichier $FICHIER n'existe pas."
    exit 1
fi

# Vérifie le type de fichier
if [ -d "$FICHIER" ]; then
    echo "Le fichier $FICHIER est un répertoire."
elif [ -f "$FICHIER" ]; then
    if [ -s "$FICHIER" ]; then
        echo "Le fichier $FICHIER est un fichier ordinaire qui n'est pas vide."
    else
        echo "Le fichier $FICHIER est un fichier ordinaire mais il est vide."
    fi
else
    echo "Le fichier $FICHIER est d'un autre type."
fi

# Affiche les permissions d'accès pour l'utilisateur courant
USER=$(whoami)

echo -n "\"$FICHIER\" est accessible par $USER en "
#vérifie en lecture
if [ -r "$FICHIER" ]; then
    echo -n "lecture "
fi
#vérifie en écriture
if [ -w "$FICHIER" ]; then
    echo -n "écriture "
fi
#vérifie en exécution
if [ -x "$FICHIER" ]; then
    echo -n "exécution"
fi

echo ""
```





### exo 4: 
```
#!/bin/bash

# Vérifie si un répertoire a été passé en paramètre
if [ "$#" -ne 1 ]; then
    echo "Erreur : Vous devez fournir un chemin de répertoire en paramètre."
    exit 1
fi

# Affiche les fichiers
echo "####### fichiers dans $1/"
#-p permet d'ajouter un << / >> aux répertoires, ensuite -grep -v sélectionne tout ceux qui ne 
# correspondent pas au motif, ici << / >>. 
ls -p "$1" | grep -v /

# Affiche les répertoires
echo "####### répertoires dans $1/"
#" ici ça n'affiche que les fichiers et répertoires n'ayant que / à la fin "
ls -p "$1" | grep /
```

### exo 5: 

```
#!/bin/bash

# Utilisation de cut pour extraire le nom d'utilisateur et l'UID

#On parcourt les utilisateurs du fichiers
for entry in $(cut -d: -f1,3 /etc/passwd); do
    # Extraire le nom et l'UID 
    user=$(echo "$entry" | cut -d: -f1)
    uid=$(echo "$entry" | cut -d: -f2)

    # Vérifier si l'UID est supérieur à 100
    if [ "$uid" -gt 100 ]; then
        echo "Utilisateur: $user, UID: $uid"
    fi
done

#!/bin/bash

# Utilisation d'awk pour afficher les utilisateurs avec un UID supérieur à 100
awk -F: '$3 > 100 {print $1, $3}' /etc/passwd
```

### exo 6: 
```                                                                                    
#!/bin/bash

# Vérification que l'utilisateur a passé des arguments
if [ $# -eq 0 ]; then
    echo "Saisissez un utilisateur valable (login ou uid)  "
    exit 1
fi

# Fonction pour vérifier par login
check_by_login() {
    local login="$1" #déclaration avec le mot clé local pour dire que la variable n'existe que dans la fonction
    user_info=$(getent passwd "$login") # getent: Récupérer des entrées depuis les bibliothèques NSS, donc on récupére les infos  de l'utilisateur 

    if [ -n "$user_info" ]; then
        user_uid=$(echo "$user_info" | cut -d: -f3) # Récupère l'UID de l'utilisateur si on a pas trouvé le login
        echo "Utilisateur trouvé: $login, UID: $user_uid"
    fi
}

# Fonction pour vérifier par UID
check_by_uid() {
    local uid="$1"
    user_info=$(getent passwd | awk -F: -v uid="$uid" '$3 == uid {print $0}') # Recherche l'utilisateur par UID

    if [ -n "$user_info" ]; then
        user_login=$(echo "$user_info" | cut -d: -f1) # Récupère le login de l'utilisateur si on a pas trouvé l'uid
        echo "Utilisateur trouvé: $user_login, UID: $uid"
    fi
}

# Vérifier si l'argument est un nombre (UID) ou un texte (login)
if [[ "$1" =~ ^[0-9]+$ ]]; then
    check_by_uid "$1"
else
    check_by_login "$1"
fi
```

### exo 7: 
```

#!/bin/bash

# Vérifier si l'utilisateur courant est root
if [ "$USER" != "root" ]; then
    echo "Ce script doit être exécuté en tant que root."
    exit 1
fi

# Fonction qui vérifie si le user existe déjà avec le login
check_by_login() {
    local login="$1"
    getent passwd "$login" > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        echo "L'utilisateur $login existe déjà."
        exit 1
    fi
}

# Fonction qui vérifie si le user existe déjà avec l'uid
check_by_uid() {
    local uid="$1"
    getent passwd | awk -F: -v uid="$uid" '$3 == uid' > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        echo "L'UID $uid est déjà utilisé."
        exit 1
    fi
}

# Fonction principale pour ajouter un utilisateur
create_user() {
    # Pose des questions pour configurer le compte utilisateur
    read -p "Entrez le login de l'utilisateur : " login
    check_by_login "$login"

    read -p "Entrez le nom de l'utilisateur : " nom
    read -p "Entrez le prénom de l'utilisateur : " prenom
    read -p "Entrez l'UID de l'utilisateur : " uid
    check_by_uid "$uid"
    
    read -p "Entrez le GID (Groupe ID) de l'utilisateur : " gid
    read -p "Entrez des commentaires pour l'utilisateur : " commentaires

    # Vérifier si le répertoire home existe déjà
    if [ -d "/home/$login" ]; then
        echo "Le répertoire /home/$login existe déjà."
        exit 1
    fi

    # Créer l'utilisateur avec useradd
    useradd -m -d "/home/$login" -u "$uid" -g "$gid" -c "$commentaires" "$login"

    # Vérifier si la création de l'utilisateur a réussi
    if [ $? -eq 0 ]; then
        echo "L'utilisateur $login a été créé avec succès."
    else
        echo "Une erreur est survenue lors de la création de l'utilisateur."
        exit 1
    fi
}

# Appel de la fonction principale
create_user
```
### exo 8: 
```



