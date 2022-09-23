Compte rendu exemple js
=============

Il y à deux fichié :

- Benford.js qui contient le programme.
- Benford.test.js qui contient les tests unitaires.

# Benford.js

```JS
const {readFileSync, promises: fsPromises} = require('fs');

//Fonction qui retourne le premier chiffre autre que zéro de la chaîne mise en paramètre.
const PremierChiffre = (chaine) =>{
    chaine = parseInt(chaine);
    return     chaine.toString()[0];
};

exports.PremierChiffre = PremierChiffre;

//Fonction qui retourne un tableau contenant tous les chiffres d'un fichier mis en paramètre.
function syncReadFile(filename) {
    const contents = readFileSync(filename, 'utf-8');
    //Ici, on sépare toutes les valeurs du fichier avec \r \n et ,
    const arr = contents.split(/\r?\n|,/);
    
    return arr;
  }

//Fonction qui retourne un tableau qui contient le nombre de fois qu'apparait chaque chiffre dans le tableau mis en paramètre.
const compteur = (tab) =>{
    let stat = [0,0,0,0,0,0,0,0,0];
    tab.forEach(Element =>{
        stat[PremierChiffre(Element)-1]++;
    });
    return stat;
}

exports.compteur = compteur;

//Fonction qui retourne un tableau contenant les pourcentages des nombres de chaque cases par rapport au total de toutes les cases additione.
const calculeStat = (tab) => {
    let stat = [0,0,0,0,0,0,0,0,0];
    let tot = tab[0]+tab[1]+tab[2]+tab[3]+tab[4]+tab[5]+tab[6]+tab[7]+tab[8];
    for(let i =0;i<9;i++){
        stat[i] = Math.round((tab[i]/tot)*100); 
    }
    return stat;
};

exports.calculeStat = calculeStat;

//On utilise la fonction syncReadFile pour lire et mettre dans le tableau data toutes les valeurs à l'intérieur.
let data = syncReadFile('./data_inv.csv');

let chiffres = [];

//On utilise la fonction PremierChiffre() pour mettre dans le tableau chiffres tous les premiers chiffres des nombres du tableau data.
data.forEach(Element =>{
    chiffres.push(PremierChiffre(Element));
});

//On utilise la fonction compteur pour mettre dans le tableau compte le nombre de fois qu'apparaît chaque chiffre du tableau chiffres.
let compte = compteur(chiffres);

//On calcule les statistiques du tableau compte avec calculeStat() et on les affiche.
console.log(calculeStat(compte));
```

# Benford.test.js

```js
const Benford= require('./Benford');

//test de la fonction PremierChiffre, on vérifie si les premiers chiffres des chaînes entrées en paramètres sont valides.
test('PremierChiffreTests', () =>{
    expect(Benford.PremierChiffre("123")).toBe("1");
    expect(Benford.PremierChiffre("3")).toBe("3");
    expect(Benford.PremierChiffre("0425")).toBe("4");
});

//test la fonction compteur, vérifie si le nombre de chiffre compté et proportionnelle au tableau entrée en paramètre.
test('compteurTests', () =>{
    expect(Benford.compteur(["3","1","2","1"])).toStrictEqual([2,1,1,0,0,0,0,0,0]);
});

//test la fonction calculeStat, vérifie si le tableau de pourcentage retourné est proportionnel au tableau mis en paramètre.
test('calculeStatTests', () =>{
    expect(Benford.calculeStat([2,1,1,0,0,0,0,0,0])).toStrictEqual([50,25,25,0,0,0,0,0,0]);
    expect(Benford.calculeStat([28,25,22,15,3,3,2,1,1])).toStrictEqual([28,25,22,15,3,3,2,1,1]);
});
```