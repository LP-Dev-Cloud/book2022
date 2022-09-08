# LP CDTL Cloud - Book 2022/2023

Cette base de connaissance est construite tout au long de l'année par les étudiants de la [LP CDTL, parcours Cloud](https://www.iut-larochelle.fr/formations/departement-informatique/licence-professionnelle-developpeur-cloud/) de l'IUT de La Rochelle, dont les enseignements sont dispensés sur le site de [Niort Tech](https://www.niort-tech.fr/). 

En collaborant durant l'année et pour chacune des unités d'enseignement, les étudiants y ajoutent les explications, exemples de code, discussions, liens et références qu'ils jugent utiles. Les contenus de ce Jupyter Book sont majoritairement rédigés en français, les échanges sur la construction de ces contenus ([dépôt Github](https://github.com/LP-Dev-Cloud/book2022)) sont quant à eux majoritaitement en anglais.

Liste des contributeurs étudiants (2022/2023) : 



Liste des contributeurs enseignants et intervenants professionnels (2022/2023) :

- [SUIRE Cyrille](https://github.com/csuire01) - La Rochelle Université

## Usage

### Building the book

If you'd like to develop on and build the LP CDTL Cloud - Book 2022, you should:

- Clone this repository and run
- Run `pip install -r requirements.txt` (it is recommended you do this within a virtual environment)
- (Recommended) Remove the existing `book/_build/` directory
- Run `jupyter-book build book/`

A fully-rendered HTML version of the book will be built in `book/_build/html/`.

### Hosting the book

The html version of the book is hosted on the `gh-pages` branch of this repo. A GitHub actions workflow has been created that automatically builds and pushes the book to this branch on a push or pull request to main.

If you wish to disable this automation, you may remove the GitHub actions workflow and build the book manually by:

- Navigating to your local build; and running,
- `ghp-import -n -p -f book/_build/html`

This will automatically push your build to the `gh-pages` branch. More information on this hosting process can be found [here](https://jupyterbook.org/publish/gh-pages.html#manually-host-your-book-with-github-pages).

## Contributors

This repository is maintained by the students of the [LP CDTL, parcours Cloud](https://www.iut-larochelle.fr/formations/departement-informatique/licence-professionnelle-developpeur-cloud/).

## Credits

This project is created using the excellent open source [Jupyter Book project](https://jupyterbook.org/) and the [executablebooks/cookiecutter-jupyter-book template](https://github.com/executablebooks/cookiecutter-jupyter-book).
