# LGM-BenchmarkToponymInterlinking
This repository contains the dataset used to train and evaluate each of the methods implemented for the Toponym Interlinking task. It also serves as an overview of those implementations by presenting a summary and the intuition of each one.

## Dataset
The dataset available here is extracted from the well known [Geonames](https://www.geonames.org/) database. It consists of a total of 5 million toponym pairs, where 2.5 million pairs are annotated as *True*, meaning that the toponyms refer to the same real world geospatial entity and the rest 2.5 million pairs annotated as *False*, indicating that the toponyms refer to different entities. This dataset was then split into 40%, 10% and 50% class-balanced parts, creating a well-formed dataset with train, validation and test subsets accordingly. This dataset provides a common ground to perform a fair comparison and a ranking of the recommended toponym interlinking methods.

## Research code implementations
- **_Similarity-ML_**: Design and implementation of custom similarity and meta-similarity string functions utilizing field knowledge to extract meaningful features. These features are then used to train a series of Machine Learning algorithms. The implementation of this method is available in the [LGM-Interlinking](https://github.com/LinkGeoML/LGM-Interlinking) repository.
- **_Toponym-Embeddings_**: A FastText embedding model is used to learn toponym embeddings which are then used as toponym representations to train and evaluate a fully-connected feed-forward network. The implementation of this method is available in the [LGM-EmbeddingToponymInterlinking](https://github.com/LinkGeoML/LGM-EmbeddingToponymInterlinking) repository.
- **_BERT_**: Finetuning BERT model on the toponym interlinking task. The implementation of this scenario is available in the [LGM-EnsembleToponymInterlinking](https://github.com/LinkGeoML/LGM-EnsembleToponymInterlinking/blob/master/1-BERT.ipynb) repository.
- **_Ensemble-methods_**: Ensemble schemes for learning to combine individual methods for the interlinking task. The implementation of this scenario is available in the [LGM-EnsembleToponymInterlinking](https://github.com/LinkGeoML/LGM-EnsembleToponymInterlinking/blob/master/2-Ensemble.ipynb) repository. Specifically, two ensemble strategies are supported:
  - *Weighted averaging*: The best weights are learnt at first on the validation set. Then, final predictions are computed as a weighted average of the individual models predictions
  - *Stacking*: In order to capture more complex combinations fo the individual models, an extra MLP model is trained on the validation predictions of the individual models and then the final predictions are made by this MLP on the test set

## Results
The following table summarizes the performance of the above methods on the specific dataset.
Method | Accuracy | Precision | Recall | F-score |
:---: | :---: | :---: | :---: | :---: |
Similarity-ML | 0.865 | 0.874 | 0.851 | 0.863 |
Siamese RNN<sup>[1](#myfootnote1)</sup> | 0.892 | 0.887 | 0.897 | 0.892 |
Toponym-Embeddings | 0.847 | 0.847 | 0.846 | 0.849 |
BERT | 0.888 | 0.872 | 0.910 | 0.890 |
Weighted avg<sup>[2](#myfootnote2)</sup> | 0.907 | **0.907** | 0.907 | 0.907 |
Stacking<sup>[2](#myfootnote2)</sup> | **0.908** | 0.902 | **0.916** | **0.909** |

<a name="myfootnote1">1</a>: Siamese RNN model for toponym interlinking as presented in R. Santos et al. "Toponym Matching Through Deep Neural Networks", 2017 and implemented [here](https://github.com/LuisPB7/StringMatching)

<a name="myfootnote2">2</a>: The individual methods utilized in the ensemble schemes are (i) Similarity-ML, (ii) Siamese RNN and (iii) BERT
