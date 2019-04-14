# Speaker Age and Gender Classification
This analysis classifies a speaker according to age and gender based on an audio recording of connected speech. The speech task is to read a paragraph of the standardized [rainbow passage](https://www.dialectsarchive.com/the-rainbow-passage). The best of 3 recordings is used for analysis.

## Data \& Preprocessing
The full data collection protocol is described by [Goy, Fernandes, Pichora-Fuller, \& van Lieshout \(2013\)](https://www.sciencedirect.com/science/article/pii/S0892199713000489). Four groups of participants were used in this analysis:

* 104 Young adult females (aged 17-27)
* 55 Young adult males (aged 17-27)
* 82 Older adult females (aged 62-81)
* 51 Older adult males (aged 64-86)

Recorded speech samples were analyzed using the [Sonneta app](https://mintleafsoftware.com/sonneta.html) to calculate 13 "bulk" features from the recording. These features include average fundamental frequency, glottal noise measures and their standard deviations over the recording, and average sound pressure level (SPL). For the full set of speech measures, see the [Sonneta documentation](http://mintleafsoftware.com/sonneta/advanced-features/speech-measures.html). The quantities SFR (dB) and SD SFR (dB) were not used as they contain information redundant with SFR (\%) and SD SFR (\%).

The data set is not publicly available.

## Model Fitting \& Validation
We compared two tree-based classifiers: **random forests** (RF) and **gradient boosting machines** (GBM). The RF models outperformed the GBM and only the RF results are tabulated below.

The random forests model has a number of hyper-parameters, 6 of which were tuned using a random search strategy (1000 parameter configurations). Once hyper-parameters were chosen, the model was validated using a **jackknife** method. This method, described in the python notebook, allows us to use the small number of samples as efficiently as possible.

## Results
The confusion matrix for the random forest classifier is as follows:
#### Confusion Matrix
```
 Real Group
Older Female  0.74  0.19  0.05  0.02
Young Female  0.19  0.80  0.00  0.00
  Older Male  0.10  0.00  0.69  0.20
  Young Male  0.05  0.00  0.15  0.80
              -- Predicted Groups --
```
In general, we see better classification on the gender axis than on the age group axis.

We can determine the importance of each feature in the model using a SHAP plot.
![alt text](https://github.com/dave-fernandes/SpeakerClassifier/blob/master/images/shap_importance.png "SHAP plot of feature importance.")

## Discussion
These results are not bad considering the simple measures that went into the analysis (no formants or unvoiced measures). This sort of classification problem usually uses Mel-frequency cepstral coefficients (MFCC) to obtain better performance. Human listeners also do a bit better than this simple approach. A re-analysis of results from [Goy, Pichora-Fuller, \& van Lieshout (2016)](https://www.ncbi.nlm.nih.gov/pubmed/27106312) show the following confusion matrices when recordings are classified by human listeners:

Classification by young listeners:
```
 Real Group
Older Female  0.76  0.23  0.01  0.00
Young Female  0.02  0.98  0.00  0.00
  Older Male  0.01  0.00  0.86  0.13
  Young Male  0.01  0.01  0.09  0.89
              -- Predicted Groups --
```

Classification by old listeners:
```
 Real Group
Older Female  0.81  0.19  0.00  0.00
Young Female  0.08  0.92  0.00  0.00
  Older Male  0.03  0.00  0.90  0.07
  Young Male  0.03  0.00  0.16  0.82
              -- Predicted Groups --
```
## Files
* `SpeakerClassifier.ipynb` is a Jupyter notebook containing the classification model, training and evaluation code.

## Implementation Notes
* Tested with Python 3.6.7, Scikit-learn 0.20.3, and shap 0.28.5
