La loi de Benford en Python
===========================

Le fichier python comporte plusieurs fonctions :
* une fonction qui récupère les données
* une fonction qui compte chaque occurence de chiffres dans un tableau
* une fonction qui donne le pourcentage de chaque occurence de la liste
* une fonction de test qui test la fonction 'compte_chiffre'

```python
# code de Ryan BOUARD
import csv
import unittest
from cgi import test
from collections import Counter


def get_data():
    number_list = []

    with open('data.csv') as csvfile:
        rdr = csv.reader(csvfile, delimiter=',')

        for row in rdr:
            number_str = row[0]
            number_list.append(number_str[0])

    return number_list

def compte_chiffre(list):
    occurence_list = Counter(list)
    print (occurence_list)
    return(occurence_list)

def pourcentage_chiffre(dict):
    somme = sum(dict.values())
    for k, v in dict.items():
        pct = v * 100 / somme
        print (k, pct)

def test_compte_chiffre(self):
    self.assertEqual(compte_chiffre([1,1,2,2,2,3,6,8,9]), {1:2, 2:3, 3:1, 6:1, 8:1, 9:1}, 'Fonction fonctionnelle !')

data = get_data()
compte_data = compte_chiffre(data)
pourcentage_chiffre(compte_data)

test_compte_chiffre
```