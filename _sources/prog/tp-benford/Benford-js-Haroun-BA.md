Loi de Benford - JS - HBA
==========================

Tout le code permettant de visualiser la Loi de Benford à partir d'un tableau de donnée est traité 
dans le fichier chiffre.js.

Les données utilisées sont dans un fichier "data_inv.csv".

Des commentaires sont ajoutés pour connaitre le rôle de chaque fonction, variables, etc...

pour pouvoir executer le code, il suffit de taper dans le terminal : node chiffre.js

```js
var _ = require('lodash');

//variables et constantes

var number = 0;
var result = 0;
var n = 0;
var tableau;

const fs = require("fs");
const { parse } = require("csv-parse");

const data = [];

//Arrow function pour calculer la somme des valeurs d'un object

let somme = tableau => Object.values(tableau).reduce((a, b) => a + b);

//On récupère les premiers chiffres

function first_digit(number) {
	number = parseInt(number, 10);
    n = number.toString().length;
    return result = Math.floor((number / Math.pow(10, n - 1)));
}

// On calcule le pourcentage de chaque chiffre.
// Le .tofixed(2) permet d'avoir 2 chiffres après la virgule pour les pourcentages.

function compteur(tableau){
  elementOccurrences = _.countBy(tableau);
  let add = somme(elementOccurrences);
  return result = Object.keys(elementOccurrences).map(k => ({[k] : (elementOccurrences[k]/add * 100).toFixed(2)}));
}

// Lecture des données et création du tableau de valeurs.
// On utilise le parseur à partir d'un pipe.

fs.createReadStream("./data_inv.csv")
  .pipe(
    parse()
  )
  .on("data", function (row) {
    //  push the object row into the array
    data.push(first_digit(row));
  })
  .on("error", function (error) {
    console.log(error.message);
  })
  .on("end", function () {
    //  log the result array
    console.log("parsed csv data:");
    console.log(data);
    console.log(compteur(data));
  })
  .on('close', () => {
      console.log("all done");
  });
```