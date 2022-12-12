# Dégémol
Détection d'un texte généré automatiquement par un modèle de langage (ex. GPT-2).

Le projet original a été porté par Hendrik Strobelt, Sebastian Gehrmann et Alexander M. Rush dans le cadre d'une collaboration entre le laboratoire MIT-IBM Watson AI Lab et HarvardNLP, sous le nom de <a href='http://gltr.io'>GLTR (Giant Language Model Test Room)</a>.

Page web du projet (en anglais) : [http://gltr.io](http://gltr.io)<br>
Démonstration (en anglais) : [http://gltr.io/dist/index.html](http://gltr.io/dist/index.html)<br>
Publication : [https://arxiv.org/abs/1906.04043](https://arxiv.org/abs/1906.04043)<br> 

## Démarrage rapide

Installer les dépendances pour Python >3.6 :

```bash
pip install -r requirements.txt
```

Lancer le serveur pour `gpt2-french-small`:

```bash
python server.py

```

L'app est accessible au lien : [http://localhost:5001/client/index.html](http://localhost:5001/client/index.html)

## Approche statistique

### Histogramme des fractions de probabilité

La web-app propose un histogramme représentant la distribution des fractions de probabilité normalisées. Pour un certain mot $p$, il s'agit de déterminer la probabilité que ce mot figure effectivement à cet emplacement étant donné son contexte (en l'occurence les mots précédents). A chaque mot du répertoire $w_i$, on associe une probabilité $x_i$ d'apparition à l'emplacement considéré. On obtient un dictionnaire de la forme :

$${w_1: x_1, w_2: x_2, ..., w_n: x_n}$$

Les $(x_i)_{1 \leq i \leq n}$ définissent une probabilité, et doivent donc satisfaire la propriété de normalisation :

$$\sum_{i=1}^n x_i = 1$$

On identifie ensuite l'indice $k$ tel que $p = w_k$. On peut alors calculer $\text{frac}(p)$ :

$$\text{frac}(p) = \frac{x_k}{\max_{1 \leq i \leq n} x_i}$$

Si le mot $p$ figure en première position des prédictions, on obtient alors $\text{frac}(p)=1$.

L'histogramme ainsi construit nous permet d'analyser de façon aggrégée les probabilités d'apparition des différents termes à leurs emplacements respectifs. Un histogramme dont la densité apparaît élevée autour de $\text{frac}(p)=1$ témoigne d'un texte hautement prévisible, et donc plus susceptible d'avoir été généré automatiquement par un modèle de langage.

## Licence

Apache 2

© 2019 par Hendrik Strobelt, Sebastian Gehrmann, Alexander M. Rush<br>
© 2022 par Pierre de Boisredon, Sylvain Delgendre, Thomas Kessous, François Schmerber
