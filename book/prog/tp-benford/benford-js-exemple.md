Compte rendu exemple js
=============

Il y à deux fichié :

- Benford.js qui contient le programme.
- Benford.test.js qui contient les tests unitaires.

# Benford.js

```JS
const {readFileSync, promises: fsPromises} = require('fs');

//fonction qui retourne le premier chiffre autre que zero de la chaine mise en parametre.
const PremierChiffre = (chaine) =>{
    chaine = parseInt(chaine);
    return     chaine.toString()[0];
};

exports.PremierChiffre = PremierChiffre;

//fonction qui retourne un tableau contenant tout les chiffre d'un fichier mis en parametre.
function syncReadFile(filename) {
    const contents = readFileSync(filename, 'utf-8');
  
    const arr = contents.split(/\r?\n|,/);
    
    return arr;
  }

//fonction qui retourne un tableau qui contient le nombre de fois qu'apait chaque chiffre dans le tableau mis en parametre.
const compteur = (tab) =>{
    let stat = [0,0,0,0,0,0,0,0,0];
    tab.forEach(Element =>{
        stat[PremierChiffre(Element)-1]++;
    });
    return stat;
}

exports.compteur = compteur;

//fonction qui retourne un tableau contenant les pourcentages des nombres de chaques cases par rapport au total de toutes les case additionée.
const calculeStat = (tab) => {
    let stat = [0,0,0,0,0,0,0,0,0];
    let tot = tab[0]+tab[1]+tab[2]+tab[3]+tab[4]+tab[5]+tab[6]+tab[7]+tab[8];
    for(let i =0;i<9;i++){
        stat[i] = Math.round((tab[i]/tot)*100); 
    }
    return stat;
};

exports.calculeStat = calculeStat;

let data = syncReadFile('./data_inv.csv');
console.log(data);

let chiffres = [];

data.forEach(Element =>{
    chiffres.push(PremierChiffre(Element));
});

let compte = compteur(chiffres);

console.log(calculeStat(compte));
```

# Benford.test.js

```js
const Benford= require('./Benford');

//test de la fonction PremierChiffre, on vérifie si les premier chiffre des chaines entrées en parametres sont valides.
test('PremierChiffreTests', () =>{
    expect(Benford.PremierChiffre("123")).toBe("1");
    expect(Benford.PremierChiffre("3")).toBe("3");
    expect(Benford.PremierChiffre("0425")).toBe("4");
});

//test la fonction compteur, vérifie si le nombre de chiffre compté et proportionelle au tableau entrée en parametre.
test('compteurTests', () =>{
    expect(Benford.compteur(["3","1","2","1"])).toStrictEqual([2,1,1,0,0,0,0,0,0]);
});

//test la fonction calculeStat, vérifie si le tableau de pourcentage retourné est proportionelle au tableau mis en parametre.
test('calculeStatTests', () =>{
    expect(Benford.calculeStat([2,1,1,0,0,0,0,0,0])).toStrictEqual([50,25,25,0,0,0,0,0,0]);
    expect(Benford.calculeStat([28,25,22,15,3,3,2,1,1])).toStrictEqual([28,25,22,15,3,3,2,1,1]);
});
```