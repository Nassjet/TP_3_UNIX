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
                                                                                               
#!/bin/bash

if [ $# -ne 2 ]; then
        echo "Veuillez saisir 1 login et 1 uid"
fi

login 

