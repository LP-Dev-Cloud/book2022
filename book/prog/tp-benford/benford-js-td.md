# JS
Thomas Dulac
---
Le code complet peut être trouvé sur la force, [ici](https://forge.iut-larochelle.fr/tdulac/Benford_js).  
Il est écrit en *javascript* et s'execute avec *nodeJs*. 

Il illustre la loi de Benford à partir d'un jeu de donnée.
Il s'agit de mesurer la fréquence de chaque premier chiffre dans une série de nombre extraite du monde réel.

Les données sont lues dans un CSV et le résultat et affiché dans la console.


## Fonctionnement général

le programme est découpé en plusieurs fonctions : 
- `main` qui execute la logique globale et utilise les autres fonctions
- `parseAsync` provient d'un plugin (node-csv) et a été transformé en *promise* via `promisify`. Elle parse un fichier CSV.
- `cleanCsv` parse en nombres la première colonne du CSV et supprime les valeurs invalides
- `firstDigit` qui prend une liste de nombre et retourne une liste de 1er chiffres significatifs
- `countFrenquensy` prend une liste de **chiffres** et retourne une liste un dictionnaire de 'chiffre:fréquence'

Les tests unitaires utilisent le module js AVA

## index.js

est le point d'entré et exécute la fonction `main`
```javascript
import { readFile } from 'node:fs'
import { promisify } from 'node:util'
import { createParser } from 'node-csv'
import cleanCsv from './clean-csv.js'
import firstDigit from './first_digit.js'
import countFrequensy from './count-frequensy.js'

//promisify method with callback
const readFileAsync = promisify(readFile)
const parseAsync = promisify(createParser().parse)

async function main() {

    // Read file
    const data = await readFileAsync("./data/data_inv.csv")

    // Parse CSV
    /**
     * @type Array<Array<string>> 
     */
    const csv = await parseAsync(data.toString())

    // Clean CSV
    const numberList = cleanCsv(csv)

    // Get first digit
    const firstDigitList = numberList.map(n => firstDigit(n))

    // Count first digit frequensy
    const firstDigitFrequensy = countFrequensy(firstDigitList);

    //print result
    Object.keys(firstDigitFrequensy).forEach(k => {
        console.log(`${k} a une fréquense de ${firstDigitFrequensy[k]}%`)
    })
}

```

## clean-csv.js
la fonction gère quelques cas particuliers : 
- quand le CSV est vide, le parseur renvoi une string vide
- quand le CSV n'a qu'une ligne le parseur renvoi un tableau de string
- quand le CSV a plusieurs ligne le parseur renvoi un tableau de tableau.


```javascript
/**
 * Transform parsed CSV to be a simple list of number
 * @param {Array<Array<string>} rawCsv 
 * @returns {Array<number>}
 */
export default function cleanCsv(rawCsv){
     
    // empty file (the csv parser return a empty string for a empty csv file)
    if(rawCsv == '') 
        return []

     // monoline file (the csv parser return a simple array for a monoline csv file)
    if(typeof rawCsv[0] === "string" )
        rawCsv = [rawCsv]

    // the csv parser return a matrix : row[column[cell]]
    return rawCsv
        .map(row=>parseInt(row[0])) // try to parse the first column in number
        .filter(n=>n !== NaN) // filter to keep only well parsed number
}
```

### tests

```javascript
import test from 'ava'

import cleanCsv from '../clean-csv.js'

test("empty", t => {
    t.deepEqual(cleanCsv(""), [])
})

test("one line", t => {
    t.deepEqual(  cleanCsv(["1"]), [1])
})

test("standard", t => {
    t.deepEqual(  cleanCsv([["1"],["2"],["3"]]), [1,2,3])
})
```

## first-digit.js
C'est une fonction récursive.
Elle utilise une fonction de 'troncage' de js.

```javascript
/**
 * Return the first signifiant digit of a number 
 * @param {number} number 
 * @param {number} pw 
 * @returns {number} 
 */
export default function firstDigit(number, pw=1){

    const divided = number / Math.pow(10,pw);

    if(divided >= 10)
        return firstDigit(number, pw+1)
    else if(divided < 1)
        return firstDigit(number, pw-1)
    
    return Math.trunc(divided)
}
```
### tests

```javascript
test("test 1", t=>{
    t.is(first_digit(1),1, "Devrait retourner 1 à partir de 1")
})

test("test 10", t=>{
    t.is(first_digit(10),1, "Devrait retourner 1 à partir de 10")
})

test("test 0.63", t=>{
    t.is(first_digit(0.63),6, "Devrait retourner 6 à partir de 0.63")
})

test("test 0.000009", t=>{
    t.is(first_digit(0.000009),9, "Devrait retourner 9 à partir de 0.000009")
})

test("test 48.3", t=>{
    t.is(first_digit(48.3),4, "Devrait retourner 4 à partir de 48.3")
})

test("test 89999.9999", t=>{
    t.is(first_digit(89999.9999),8, "Devrait retourner 8 à partir de 89999.9999")
})

test("test 0.0008999", t=>{
    t.is(first_digit(0.0008999),8, "Devrait retourner 8 à partir de 0.0008999")
})

test("test 10000000", t=>{
    t.is(first_digit(10000000),1, "Devrait retourner 1 à partir de 10000000")
})
```

## count-frenquensy.js
Retourne un object dont les clés sont les chiffres et les valeurs leurs fréquences

```javascript
/**
 * From a array of digit, return a object with digit in key and frenquensy in value
 * @param {Array<number>} digitList 
 * @returns {Object}
 */
export default function countFrequensy(digitList) {

    const digitCount = [0, 0, 0, 0, 0, 0, 0, 0, 0]

    // count digit
    digitList.forEach(n => {
        if (n < 1 || n > 9)
            throw new Error("invalid digit")

        digitCount[n - 1]++
    })

    // calc frequensy
    const digitFrequensy = {}

    digitCount.forEach((d, index) => {
        if (d == 0) return
        digitFrequensy[(index + 1).toString()] = Math.round(d * 1000 / digitList.length) / 10
    })


    return digitFrequensy
}
```
### tests

```javascript
import test from 'ava'

import countFrequensy from '../count-frequensy.js'

test("not digit error", t => {
    t.throws(() => countFrequensy([10, 0, 99]), { message: "invalid digit" }, "must throw error if number is not 0<x<10")
})

test("liste vide", t => {
    t.deepEqual(countFrequensy([]), {}, "empty list must result in empty object")
})

test("two one", t => {
    t.deepEqual(countFrequensy([1, 1]), { 1: 100 }, "the result is not correct")
})

test("one two three", t => {
    t.deepEqual(countFrequensy([1, 2, 3]), { 1: 33.3, 2: 33.3, 3: 33.3 }, "the result is not correct")
})
```


