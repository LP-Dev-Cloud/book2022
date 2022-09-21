# Contenu de main.py :

```python

def read_data():
    with open("data.csv", "r") as fichier:                  # le fichier data.csv s'ouvre en "read only"
        lesLignes = fichier.readlines()                     # récupère les lignes du fichiers à travers une liste

  fichier.close()                                           # fermeture de la lecture du fichier

  return lesLignes                                          # retourne la liste des lignes

def substring(text):
  text = f"{text}"                                          # transforme la variable texte pour être sûr que c'est une chaine de caractère
  i = 0
  trouve = False

  while ((not trouve) and (i < len(text))):                 # tant qu'il n'a pas trouvé un nombre différent de zero et que le compteur n'est pas arrivé à la fin de la longueur de la ligne -> text étant ici la variable général puis sera replacer par les lignes du fichier
    if (text[i] != '0'):                                    # si l'element au compteur est différent de 0, alors on a trouvé le premier nombre; celui qui nous interesse ici
      trouve = True
    else:
      i += 1                                                # sinon, on rajoute 1 au compteur

  return text[i]                                            # retourne le dernier index qui est celui du premier nombre différent de zero; celui qui nous intéresse. En effet, le but est uniquement d'obtenir le premier nombre.

def check(element, liste):                                  # cette fonction est de vérifier si un element est dans une liste
  i = 0
  trouve = False

  while ((not trouve) and (i < len(liste))):
    sous_liste = liste[i]
    if (element == sous_liste[0]):
      trouve = True
    else:
      i += 1

  return trouve

def add_liste(element, liste):                               # cette fonction rajoute une valeur (element) dans une liste. Ici, valeur de l'element += 1. Par exemple : [2,1] donnera [2,2]. Dans le programme, cette fonction sers à compter le nombre de fois qu'il y a un nombre dans ce sens la : il y a [.,1] [2,.]
  i = 0
  trouve = False

  while ((not trouve) and (i < len(liste))):
    sous_liste = liste[i]
    if (sous_liste[0] == element):
      sous_liste[1] += 1
      trouve = True
    else:
      i += 1

  return liste

def countNb(liste):
  lg_liste = len(liste)
  nb_liste = []

  for i in range(1,lg_liste):                                   # on fait une boucle de 1 jusqu'à la longueur de la liste qui représente ici les lignes dans le csv. Partir de 1 est important car la première ligne (le premier index de la liste) s'affiche différement et ne peut être traité.
    if (check(int(substring(liste[i])),nb_liste)):              # on vérifie que le nombre existe déjà
      nb_liste = add_liste(int(substring(liste[i])), nb_liste)  # si c'est le cas, on lui ajoute + 1
    else:
      nb_liste.append([int(substring(liste[i])),1])             # sinon, on rajoute le couple [nombre, 1]

  return nb_liste

def pourcentage(liste, total):                                  # cette fonction sert à rajouter le pourcentage pour chaque nombre
  lg_liste = len(liste)

  for i in range(lg_liste):
    sous_liste = liste[i]
    sous_liste[1] = round((sous_liste[1] / total) * 100)

  return liste

datas = read_data()
print(pourcentage(countNb(datas),len(datas)))
```