Javascript - exemple Loi Benford
================================

Le code est décomposé en deux principaux fichiers : 

`Test.js` qui contient la fonction `main` et la fonction permettant de parser un fichier CSV et en extraire les données. Plusieurs opérations sont effectués afin de changer le type des résultats trouvé pour les mettre dans un tableau.


```javascript
//Test.js

var nombre = 1593 ;
var result = 0 ;
var tab = [] ;

```
Fonction permettant de parser un fichier CSV et en extraire les données.

```javascript

const fs = require("fs");
const { parse } = require("csv-parse");

fs.createReadStream("./data.csv");

fs.createReadStream("./data.csv")
  .pipe(parse())
  .on("data", function (row) 

```
Conversion de type de données

```javascript
  {

    
    var To_Int = parseInt(row) ;
    var To_String = To_Int.toString();
    var first_digit = To_String.charAt(0);
    tab.push(first_digit);
    //console.log(first_digit);
  })
  .on("error", function (error) 
  {
    console.log(error.message);
  })
  .on("end", function () 

```
Affichage des résultat

```javascript
  {
    console.log(tab);
    
    console.log("fin");
  });

result = parseInt(nombre, 10) ;
console.log(result) ;