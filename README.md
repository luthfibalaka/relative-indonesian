## Relative Package

This repository hosts the code to generate word embeddings for the paper *[A Latent Variable Model for Learning Distributional Relation Vectors](http://josecamachocollados.com/papers/relative_ijcai2019.pdf)* (IJCAI 2019). This is actually a copy of the [original repository](https://github.com/pedrada88/relative), but I adapted it to use modern packages.

Sources:
- [Dataset](https://www.kaggle.com/datasets/mikeortman/wikipedia-sentences)

Run the code with:

nohup python -u relative_init.py -corpus wiki_clean.txt -embeddings cc.en.300.bin -output relative_init_vectors.txt


## Info from original README

### Quick start: Get your own relation embeddings

```bash
python relative_init.py -corpus INPUT_CORPUS -embeddings INPUT_WORDEMBEDDINGS -output OUTPUT_RELATIVE_EMBEDDINGS
```

With this command you can directly get your relation embeddings given a tokenized corpus as input. You can use **any word embeddings as input**, either in *txt* or *bin* formats (those accepted by gensim). The input word embeddings can be trained on the same corpus (as in the reference paper) or pre-trained on a different corpus.

**Execution time**: This will very much depend on the size of the corpus and other factors (computer power, parameters, etc.). As an indication, running the default code (as above) in a normal CPU with default parameters on the whole Wikipedia, the total running time is about six hours.

#### Example:

```bash
python relative_init.py -corpus wikipedia_en_preprocessed.txt -embeddings fasttext_wikipedia_en_300d.bin -output relative_init_vectors.txt
```

### Parameters

A number of optional parameters can be specified to your needs: 

*-contexts*: Path of the input contexts directory, so these do not need to be re-compiled (output of "context_extraction.py").

*-norm*: Output vectors normalized ("true") or not ("false"). Default: false

*-wordfreq*: Path of the frequency dictionary file.

*-window*: Co-ocurring window size. Default: 10

*-min_freq_cooc*: Minimum frequency of words between word pair: increasing the number can speed up the calculations and reduce memory but we would recommend keeping this number low. Default: 1

*-pairvocab*: Path of the input pair vocabulary file (tab-separated with at least two columns, one word pair per line).

*-wordsize*: Maximum number of words considered (sorted by frequency). Default: 100000

*-output_pairvocab*: Co-ocurring window size. Default: 10

*-stopwords*: Path to stopwords file ("false" if no stopwords to be used). Default: stopwords_en.txt

*-min_freq*: Minimum frequency of words. Default: 5

*-smoothing*: Alpha smoothing factor in the pmi calculation. Default: 1

*-min_occ*: Minimum number of occurrences required for word pairs. Default: 20

*-max_pairsize*: Maximum number of word pairs. Default: 3000000

*-symmetry*: Indicates whether pairs are symmetric (true) or not (false). Default: false

#### Example:

For example, if you would like to give your own pair vocabulary as input and specify a shorter window size to 5 (instead of the default 10), you can type the following:

```bash
python relative_init.py -corpus wikipedia_en_preprocessed.txt -embeddings fasttext_wikipedia_en_300d.bin -output relative_init_vectors.txt -pairvocab pair_vocab.txt -window 5 
```

### Working step by step

It is also possible to run relative-init step by step:

1. *get_vocabulary.py*: This script outputs word vocabulary ("word_vocab.txt"), pair vocabulary ("pair_vocab_pmi.txt") and word frequency dictionary ("word_frequency_all.txt"), computed on the input corpus.
2. *context_extraction.py*: This script extracts contexts for word pairs from the input corpus given a pair vocabulary file.
3. *relative_init.py*: Instead of starting from scratch, contexts from the previous step can be directly provided.

Parameters for each of these steps are similar to the ones indicated above, check documentation in the script if in doubt. Some important tips/notes when working with the code:

*Tip 1:* If you have memory issues when running the code, you can split your pair vocabulary files in several files and concatenate the resulting output vectors (computation is done independently for each pair).

*Tip 2:* If you would like to optimize for speed, you can play with the parameters by e.g. reducing the window size, max_pairsize or wordsize, or by augmenting min_occ, min_freq or min_freq_cooc. This will also have an effect on a reduced memory workload.

*Tip 3:* Depending on the size of your input corpus, you may want to increase the min_occ parameter (if the corpus is large) or decrease it (if the corpus is small). Similarly you can play with the wordsize and max_pairsize parameters.


### Reference paper

If you use any of these resources, please cite the following [paper](http://josecamachocollados.com/papers/relative_ijcai2019.pdf):
```bash
@InProceedings{camachocollados:ijcai2019relative,
  author = 	"Camacho-Collados, Jose and Espinosa-Anke, Luis and Shoaib, Jameel and Schockaert, Steven",
  title = 	"A Latent Variable Model for Learning Distributional Relation Vectors",
  booktitle = 	"Proceedings of IJCAI",
  year = 	"2019",
  location = 	"Macau, China"
}

```
If you use FastText, please also cite its corresponding paper.

License
-------

Code and data in this repository are released open-source.

Copyright (C) 2019, Jose Camacho Collados.
