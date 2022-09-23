PHP - Loi de Benford 
================
Blandin Geremy

La loi de benford, selon laquelle les nombres que l'on rencontre dans la vie quotidienne commenceraient beaucoup plus souvent par le chiffre 1 que par le chiffre 9.

Le code est décomposé en deux principaux fichiers et un dossier : 

- 1 : index.html 
- 2 : testBenford.php
- 3 : dossierCSV

Le principe de l'application web :

- Le menu HTML en cliquant sur le boutton "GO" envoie à la page de traitement,

- Le fichier php va recuperer tous les fichiers CSV present dans le dossier "dossierCSV" et effectuer le traitement afin d'avoir le resultat premettant de verifier la loi de benford.


`index.html` qui contient le menu `index` et la fonction permettant d'appeler la page de traitement php.

```rust
//menu
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>


<div class="d-flex justify-content-center">
    <form action="testBenford.php" method="POST">
        <h5>Menu</h5>
        <br>
        <button type="submit" class="btn btn-primary">GO</button>
        </div>
    </form>
</div>
// Bouton qui permet le lancement du traitement PHP
```

`testBenford.php` qui contient le traitement php qui permettra de mettre en place la "loi de Benford"

```rust
<?php
error_reporting(E_ALL);
ini_set("display_errors", 1);

       // SI AJOUT DE FICHIER ET QUE LES RESULTATS SUR PAGE WEB RESTE IDENTIQUES FAIRE ctrl + f5
       //Lister tout
       $scandir = scandir(".\\dossierCSV\\");
       $ligne = 0;
       foreach($scandir as $fichier){

              $csv[$ligne] = $fichier;
              $ligne++ ;
              
       }

       //lecture des fichiers csv
       $i = 2;
       while($i < count($csv)){
  
              //ouvre les csv present dans le dossier
              $file = fopen(".\\dossierCSV\\".$csv[$i],"r");
        
              //recupere toute les données des csv
              while(! feof($file)){

                     $val = fgetcsv($file);
                     $converted[] = array_map('intval', $val);
              }
       $i++;
       }

       $nbreDeFichier = $i -2; // -2 car (./ et ../)

       //recup premier nombre
       $a=0;
       while($a < count($converted)){

              if($converted[$a][0] != 0){
                     $valSansVirgule = $converted[$a][0];
                     $firstNbr[] = substr($valSansVirgule, 0, 1);

                     $nbrNumTab = count($firstNbr); //nbre total 
                     $key = array_count_values($firstNbr); //recup nbre identique                   
              }
       $a++;
       }

       //calcul pourcentage
       $b = 1;
       while($b <= 9){
              if(!empty($key[$b])){
                     $calculPourcentage[$b] = $key[$b] / $nbrNumTab *100;
              }
              $b++;
              
       }
?>
//affichage du resultat
<html>
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
      <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

       <br>
       <div class="d-flex justify-content-center">
              <div class="card text-center shadow bg-white " style="width: 40rem;">
                     <div class="card-body">
                            
                            <h5 class="card-title">Resultat de tous les fichiers .csv present</h5>
                            <h6 class="card-subtitle mb-2 text-muted">fichiers present dans "dossierCSV"<br>Actuellement : <?php echo $nbreDeFichier?></h6>
                            <?php $i=1; while($i <= count($calculPourcentage)){ ?>
                                   <p class="list-group-item">Pourcentage  de <?php echo $i ?> : <?php if(!empty($calculPourcentage[$i])){ echo $calculPourcentage[$i];}else{ echo "0";} ?></p>
                            <?php $i++; } ?>

                            <form action="index.html" method="POST">
                                   <button type="submit" class="btn btn-primary">Retour menu</button>
                            </div>
                     </div>
              </div>
              <br>
       </div>
</html>
```
