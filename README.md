# Novozyme Enzyme Stability

[*Adapted from the original Kaggle competition.*](https://www.kaggle.com/competitions/novozymes-enzyme-stability-prediction)

Enzymes are proteins that act as catalysts in the chemical reactions of living organisms. The goal of this competition is to predict the thermostability of enzyme variants. The experimentally measured thermostability (melting temperature) data includes natural sequences, as well as engineered sequences with single or multiple mutations upon the natural sequences.

Understanding and accurately predict protein stability is a fundamental problem in biotechnology. Its applications include enzyme engineering for addressing the world’s challenges in sustainability, carbon neutrality and more. Improvements to enzyme stability could lower costs and increase the speed scientists can iterate on concepts.

## Data Cleaning
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme01.png)
<br>
Four initial features:
- `seq_id` (index) – *Sequence Identification Number*
- `protein_sequence` – *Amino Acid Sequence*
- `pH` – *Measured pH*
- `data_source` – *Source of the Experimental Data*
- `tm` – *Target Column (Proportional to Stability)*
<br>
The first task in order to clean this data is to determine the inconsistencies involving the entries for pH. There are 286 missing values. Based on the total number of values involved, I felt comfortable just eliminating those instances that had no corresponding pH measurement.
<br>
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme02.png)
<br>
Another issue involving pH was the maximum value for pH: 64.9. Values of pH typically range between 0 and 14.
<br>
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme03.png)
<br>
Investigating the issue found that these anomalous values seemed to occur due to a misplacement of the decimal point. Corrective action was taken to salvage the remaining data.
<br>
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme04.png)

## Feature Investigation
As `pH` and `protein_sequence` are the only workable features to introduce into a machine learning model, I needed to investigate further to create more differentiable features. Two ideas came to mind dealing with the string value of `protein_sequence`. I was able to compare the **Levenshtein Distance** between the given 'wild type' enzyme and the `protein_sequence`. I found that using **Biopython** to analyze the sequence proved more effective in providing useful information to base my model off of:
Five additional features:
- `comp_score` - Levenshtein Distance from the 'wild type'
- `mol_wt` - Molecular Weight
- `instability` - Instability Index
- `gravy` - Gravy
- `aromaticity` - Aromaticity
<br>
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme05.png)
<br>
The Pearson Corrolation Matrix is as follows:
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme06.png)

## Picking a Machine Learning Model
In order to narrow down the type of model to use, I used a python library called 'lazypredict'. After dividing the data into seperate training and test sets, I tested and graphed the different untuned models based on their R^2^ metric:
<br>
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme07.png)
<br>
After tuning the top three models, I found that LightGBM provided a marginally better test score than the HGBR Tree model:
<br>
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme08.png)
<br>

## Results
The resultant model's Spearman Correlation Coefficient to the unseen Kaggle data is below:
<br>
![Image](https://github.com/MarcJKolb/Novozyme-Enzyme-Stability/blob/master/Images/readme09.png)
<br>