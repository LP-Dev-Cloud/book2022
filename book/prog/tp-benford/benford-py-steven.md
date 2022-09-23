# Loi de Benford - BRAY Steven

La loi de Benford est une loi mathématique qui prouve que le premier chiffre significatif d'un nombre d'une liste de donnée de la vie de tous les jours (exemple: Population d'un pays) le chiffre 1 est plus fréquent que le 2, lui-même plus fréquent que le 3, etc ...

L'objectif de ce TP est de prouver cette loi grace à une liste donnée en CSV (ici, nous testons une liste du nombre de population dans chaque commune des Deux-Sèvres). Pour faire ce programme, j'ai choisi de le faire en Python.

Le programme entier et le CSV sont disponible en [cliquant ici](https://forge.iut-larochelle.fr/sbray/benford_py).

## Import de librairie

Pour pouvoir travailler avec un CSV, en python, il faut importer la librairie "csv" qui est déjà préinstallée dans Python.

```python
import csv
```

## Ouverture et lecture du fichier CSV

La fonction open_csv, comme son nom l'indique, sert à ouvrir le fichier CSV dont on veut traiter la loi de Benford. Les données du CSV sont stockées dans une liste (que je convertis en integer car les données sont sous forme string) nommée list_number afin de l'envoyé dans la fonction benford.

```python
def open_csv():
    filename = "./csv/data.csv"

    with open(filename, newline='') as f:
        list_number = []
        read = csv.reader(f)
        for line in read:
            list_number.append(int(line[0]))
    
    benford(list_number)
```

## Traitement de la loi de Benford

La fonction benford, comme son nom l'indique, traite la loi de Benford, je lui donne une liste de nombre en paramètres de celle-ci. 

Dans le dict_number, c'est là où seront stockés les chiffres (en clé) et le nombre d'occurrences dans le CSV (en valeur). Je boucle dans la liste de nombre et je mets dans la variable number le premier chiffre du nombre n de la liste. J'expliquerais juste après le déroulement de la fonction first_digit. 

Ensuite, je vérifie si le chiffre existe dans le dictionnaire, si oui je rajoute 1 à l'occurence de chiffre, sinon il ajoute le chiffre au dictionnaire et lui donne 1. 

Enfin, je boucle dans le dictionnaire dict_number et je calcule en pourcentage le nombre d'occurrences de chaque chiffre, et j'affiche le dictionnaire dict_benford dans la console.

```python
def benford(list_number):

    dict_number = {}

    for index in range(len(list_number)):
        number = first_digit(list_number[index])

        if number in dict_number:
            dict_number[number] += 1
        else:
            dict_number[number] = 1
    
    dict_benford = {}

    for index in range(len(dict_number)):
        dict_benford[index + 1] = str(round((dict_number[index + 1]*100)/len(list_number))) + "%"

    print(dict_benford)
```

La fonction first_digit permet, en faisant une succession de division par 10, de données le premier chiffre significatif d'un nombre. Si le nombre commence par 0 ou est égal à 0, celui-ci n'est pas traité.

```python
def first_digit(number):

    if number >= 10:
        
        while number >= 10:
            number = number / 10
        
        return int(number)

    elif number < 10 and number >= 1:
        return int(number)
```

## Appel de la fonction open_csv

```python
if __name__ == '__main__':
    open_csv()
```

## Résultat du programme

```json
{1: '30%', 2: '17%', 3: '12%', 4: '7%', 5: '8%', 6: '8%', 7: '7%', 8: '7%', 9: '5%'}
```
## Test unitaire

Ici, je veux tester ma fonction first_digit du programme précédent. Je veux savoir si ma fonction retourne le premier chiffre significatif de chaque nombre. Pour faire des tests unitaires, en Python, il y a déjà une librairie `code` qui est préinstallée

```python
from benford import first_digit
import unittest

class TestStringMethods(unittest.TestCase):

    def test_firstDigit(self):
        self.assertEqual(first_digit(4565615615), 4)
        self.assertEqual(first_digit(3123815561), 3)
        self.assertEqual(first_digit(5521312345), 5)
        self.assertEqual(first_digit(65656111), 6)
        self.assertEqual(first_digit(2), 2)
        self.assertEqual(first_digit(956456456), 9)
        self.assertEqual(first_digit(4451212135), 4)
        self.assertEqual(first_digit(245645648), 2)
        self.assertEqual(first_digit(812312312), 8)
        self.assertEqual(first_digit(71231156), 7)
        self.assertEqual(first_digit(45), 4)
        self.assertEqual(first_digit(682), 6)
        self.assertEqual(first_digit(100), 1)

if __name__ == '__main__':
    unittest.main()
```