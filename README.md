# Sentiment Analysis on Crypto Tweets
Model and preprocessin code used were made by and adapted from https://github.com/abdulfatir/twitter-sentiment-analysis/.


## Requirements
- `Python3`
- `TensorFlow`
- `Keras`
- `Scipy`
- `Scikit-Learn`
- `NLTK`
- `Tweepy`


## Usage

### Dataset

The training dataset is expected to be a csv file of type `tweet_id,sentiment,tweet`
where the `tweet_id` is a unique integer identifying the tweet, `sentiment`
is either `1` (positive) or `0` (negative), and `tweet` is the tweet enclosed in `""`.
Similarly, the test dataset is a csv file of type `tweet_id,tweet`.
Please note that csv headers are not expected and should be removed from the
training and test datasets.

1. Download the Twitter training corpus dataset:
`wget http://thinknook.com/wp-content/uploads/2012/09/Sentiment-Analysis-Dataset.zip -O dataset.zip`
2. Modify the dataset according to the description above.
3. Create a `dataset` directory and run `split-data.py <dataset.csv>`. It'll
split the data into a default split of 90% training and 10% test datasets. The split percentage can be defined as the third argument if desired e.g. `$python3 split-data.py dataset.csv 0.2`.
4. Download GloVe pre-trained word vectors, unzip it and and rename
`glove.twitter.27B.200d.txt` into `glove-seeds.txt` placing it inside `dataset`.
These pre-trained word vectors will be used when training our network:
`wget http://nlp.stanford.edu/data/wordvecs/glove.twitter.27B.zip`
5. Insert your Twitter API keys in `twitterAPI.py` 

Make sure all data is properly modified according to the description provided,
split and located in `/dataset`.
For more details or to change names/directories check `lstm.py`.

### Preprocessing:

1. Run `preprocess.py <raw-csv-path>` on both train and test data. This will
generate a preprocessed version of the dataset.
3. Run `stats.py <preprocessed-csv-path>` where `<preprocessed-csv-path>` is the
path of (the labeled dataset) csv generated from `preprocess.py`.
This gives general statistical information about the dataset and will two pickle files which
are the frequency distribution of unigrams and bigrams in the training dataset.

After the above steps, you should have four files in total:
`<preprocessed-train-csv>`, `<preprocessed-test-csv>`, `<freqdist>`,
and `<freqdist-bi>` which are preprocessed train dataset, preprocessed test dataset,
frequency distribution of unigrams and frequency distribution of bigrams respectively.

### Training:

1. Create a `/models` directory where the trained models will be stored.
2. Run `python3 lstm.py dataset/train-processed.csv`. It's advised to train
this model on GPUs or else it'll take several hours if trained on CPUs.

### Testing:

1. To test your model in the test data run `python3 lstm.py models/<model>
dataset/<test-processed.csv>`.
The output will be a CSV file saved into `results` which you also need to create.
In this case the file will be named "analysis-test-process.csv".
2. After that you have a trained model you could predict on any data formatted as described above
as well as fetch tweets with a given query using `twitterAPI.py`, preprocess it and
run your model over it. Remember to change the files directories defined on `lstm.py` accordingly.

