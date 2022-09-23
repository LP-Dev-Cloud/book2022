La loi de benford en python
===========================

Le code est situé dans un seul fichier 

`test2.py` qui contient le script. 
On importe en premier le csv (Comma Separated Values) qui va permettre de lire le fichier `data.csv` qui contient la base de données des nombres à analyser.
Ensuite j'importe depuis la librairie collections le Counter, il sert à compter des objets hachables. 
Puis j'ai importé le framework unittest qui sert aux tests unitaires.

J'ai fais une fonction get_data() qui contient une variable number_list vide car elle va contenir les valeurs de la base de données.
Ensuite j'ai ouvert la base de données et j'ai crée une boucle qui récupère tout les premiers chiffres sauf si ce chiffre est un 0, il prendra donc le prochain chiffre.
Après on compte avec la fonction Count le nombre de chaque chiffre, par exemple 77 chiffre 1, 56 chiffre 2 etc. 
```python
// test2.py
import csv
from collections import Counter
from unittest import result

def get_data():
    number_list = []
    with open('data.csv', newline='') as csvfile:
        rdr = csv.reader(csvfile, delimiter=',')
        for row in rdr:
            number_str = row[0]
            # TODO strip les premiers caractères selon un pattern 
            number_list.append(number_str[0])
    return number_list
        

results = get_data()


#print(results)
countresult = Counter(results)
print(Counter(results))

number_of_elements = len(results)

print(number_of_elements)

```