Rust - example 1
================

Le code est décomposé en deux principaux fichiers : 

`main.rs` qui contient la fonction `main` et la fonction permettant de parser un fichier CSV et en extraire les données. Plusieurs implémentations sont proposées avec différentes méthodes de gestion des erreurs de lecture du fichier CSV :

- 1 : Sans gestion d'erreur 
- 2 : Avec une gestion de l'erreur dans la fonction get_data
- 3 : Avec une gestion d'erreur récupérée depuis la fonction main (donnant l'opportunité de réagir)

```rust
// main.rs
mod utils;

// use std::error::Error;
use csv;
use std::{io, process};
use std::error::Error;
use crate::utils::{count_digits, get_first_digit};


fn main() {


    // Without Error handling
    let values = get_data();
    let values_by_digits = count_digits(values);
    println!("{:?}",values_by_digits); 
    // With Error handling
    // match get_data() {
    //     Ok(values) => {
    //         let values_by_digits = count_digits(values);
    //         println!("{:?}",values_by_digits); 
    //     }
    //     Err(err) =>{
    //         println!("{}", err);
    //         process::exit(1);
    //     }
    // }

}



// 1 : Read CSV Data and return a Vec
// TODO : error handling
fn get_data() -> Vec<u32> {
    let mut values:Vec<u32> = Vec::new();
    // Build the CSV reader and iterate over each record.
    let mut rdr = csv::Reader::from_reader(io::stdin());
    for result in rdr.records() {
        // The iterator yields Result<StringRecord, Error>
        // the expect method (defined on Result) unwrap the success value
        let record = result.expect("a CSV record"); // this may cause predictable error ...
        for field in &record{
            let first_digit = get_first_digit(field);
            // pushing first digit to values Vec
            values.push(first_digit);
        }

    }
    values
}

// 2 : With error handling using pattern matching
// fn get_data() -> Vec<u32>{
//     let mut values:Vec<u32> = Vec::new();
//     let mut rdr = csv::Reader::from_reader(io::stdin());
//     // rdr.records returns an iterator that yields Result that contains a record or an Error
//     for result in rdr.records() {
//         // Math the result, if it is OK, let's go
//         // if not, print a nice message and exit.
//         match result {
//             Ok(record) => {
//                 for field in &record{
//                     let first_digit = get_first_digit(field);
//                     values.push(first_digit);
//                 }
//             },
//             Err(err) => {
//                 println!("error reading CSV from <stdin>: {}", err);
//                 process::exit(1);
//             }
//         }
//     }
//     values
// }

// 3 : With Box Dyn Error handling 
// fn get_data() -> Result<Vec<u32>, Box<dyn Error>> {
//     let mut values:Vec<u32> = Vec::new();
//     let mut rdr = csv::Reader::from_reader(io::stdin());
//
//     for result in rdr.records() {
//         // This is effectively the same code as the match
//         // ? -> forward the Error to its caller
//         // ? -> is working only in function that returns a Result
//         let record = result?;
//
//         for field in &record{
//             let first_digit = get_first_digit(field);
//             values.push(first_digit);
//         }
//     }
//     Ok(values)
// }

```

Le fichier utils.rs, contient lui les deux fonctions permettant d'isoler le premier chiffre significatif, et compter le nombre de chiffre dans chaque nombre. Il propose également les tests associés à ces deux fonctions. 

```rust
// utils.rs
use std::collections::HashMap;
use std::hash::Hash;
use itertools::Itertools;


pub fn get_first_digit(number_str:&str) -> u32{
    let base:u32 = 10;
    let stripped_number_str = number_str.trim_start_matches('0');
    // Number of digits
    let number_of_digits = stripped_number_str.len();
    // cast &str to u32 (unwrap this Result type)
    let number: u32 = number_str.parse().unwrap();
    // divide by 10^number-1 to get the first digit
    let result = number / base.pow((number_of_digits - 1) as u32);
    result
}

pub fn count_digits(digits:Vec<u32>) -> HashMap<u32, usize>{
    let counts = digits.into_iter().counts();
    counts
}


#[cfg(test)]
mod test{
    use super::*;

    #[test]
    fn test_get_first_digit(){
        assert_eq!(get_first_digit("345"),3);
        assert_eq!(get_first_digit("48945"),4);
        assert_eq!(get_first_digit("17"),1);
        assert_eq!(get_first_digit("1"),1);
        assert_eq!(get_first_digit("00300"),3);
    }

    #[test]
    // test some specific key of the map like counts[1] = x
    fn test_count_digits(){
        let vec_test = vec![3,1,1,2,3,3,4];
        let mut hashmap = HashMap::new();
        hashmap.insert(1,2);
        hashmap.insert(2,1);
        hashmap.insert(3,3);
        hashmap.insert(4,1);
        assert_eq!(count_digits(vec_test),hashmap);
    }

}
```

Le format d'entrée attendu est similaire à : 

```csv
900
51479
23985
340
...
```

Une fois compilé, le programme récupère le CSV depuis `stdin` et peut être lancé avec `./programme < data.csv`

