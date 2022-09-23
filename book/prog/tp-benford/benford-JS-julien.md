Benford en version JavaScript par Julien Beauche
================

Le code javascript est composé d'une bibliotéque "lodash" ce qui permet en soit de facilite JavaScript en simplifiant le travail avec des tableaux, des nombres, des objets, des chaînes.
Il y a une fonction compteur tableau ce qui permet au logiciel de compter le nombre de catactère qu'il ya dans le tableau
Pour ce faire il faut que dans le code je puisse récupérer le fichier csv qui contient toutes les informations nécéssaire pour faire fonctionner le logiciel et mettre en oeuvre la lois de Benford
Il y a également une fonction qui donne le pourcentage de chaque occurence de la liste csv



```javascript
var _ = require('lodash');

var nombre = 0;
var resultat = 0;
var n = 0;
var tableau;

const fs = require("fs");
const { parse } = require("csv-parse");

const data = [];


function first_digit(nombre) {
	nombre = parseInt(nombre, 10);
    n = nombre.toString().length;
    return resultat = Math.floor((nombre / Math.pow(10, n - 1)));
}

function compteur(tableau){
  return elementOccurrences = _.countBy(tableau);
}



fs.createReadStream("./data_inv.csv")
  .pipe(
    parse()
  )
  .on("data", function (row) {
   
    data.push(first_digit(row));
  })
  .on("error", function (error) {
    console.log(error.message);
  })
  .on("end", function () {
 
    console.log("parsed csv data:");
    console.log(data);
    console.log(compteur(data));
  })
  .on('close', () => {
      console.log("all done");
  });

```

Une fois compilé, le programme récupère le CSV depuis `stdin` et peut être lancé avec `./programme < data.csv`

