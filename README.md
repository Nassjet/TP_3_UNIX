# TP_3_UNIX

### Exercice : paramètres

Pour l'exercice on cherche il faut afficher les paramètres qu'on aura écrit en executant le script bash. Après quelques recherches avec une recherche google on trouve que simplement un `echo $0` affiche le nom du script, du coup je mets ça : 

![image](https://github.com/user-attachments/assets/2853a950-6434-496f-987c-db176dc418dc)

La première ligne s'appelle le "shebang" c'est important pour executer n'importe quelle script, ici du bash. 
Aussi, on a déclaré une variable vide `param = $#` pour stocker les arguments passés au script (cf le cours du tp3). 

