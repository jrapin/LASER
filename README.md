# LASER  Language-Agnostic SEntence Representations

LASER is a library to calculate and use multilingual sentence embeddings.

**NEWS**
* 2019/03/18 switch to BSD license
* 2019/02/13 The code to perform bitext mining is [**now available**](tasks/bucc)

**CURRENT VERSION:**
* We now provide an encoder which was trained on [**93 languages**](#supported-languages), written in 23 different alphabets [6].
  This includes all European languages, many Asian and Indian languages, Arabic, Persian, Hebrew, ...,
  as well as various minority languages and dialects.
* We provide a [*test set for more than 100 languages*](data/tatoeba/v1)
  based on the [*Tatoeba corpus*](https://tatoeba.org/eng).
* Switch to PyTorch 1.0

All these languages are encoded by the same BiLSTM encoder, and there is no need
to specify the input language (but tokenization is language specific).
According to our experience, the sentence encoder also supports code-switching, i.e.
the same sentences can contain words in several different languages.

We have also some evidence that the encoder can generalizes to other
languages which have not been seen during training, but which are in
a language family which is covered by other languages.

A detailed description how the multilingual sentence embeddings are trained can
be found in [6], together with an extensive experimental evaluation.

## Dependencies
* Python 3.6
* [PyTorch 1.0](http://pytorch.org/)
* [NumPy](http://www.numpy.org/), tested with 1.15.4
* [Cython](https://pypi.org/project/Cython/), needed by Python wrapper of FastBPE, tested with 0.29.6
* [Faiss](https://github.com/facebookresearch/faiss), for fast similarity search and bitext mining
* [transliterate 1.10.2](https://pypi.org/project/transliterate), only used for Greek (`pip install transliterate`)
* [jieba 0.39](https://pypi.org/project/jieba/), Chinese segmenter (`pip install jieba`)
* [mecab 0.996](https://pypi.org/project/JapaneseTokenizer/), Japanese segmenter
* tokenization from the Moses encoder (installed automatically)
* [FastBPE](https://github.com/glample/fastBPE), fast C++ implementation of byte-pair encoding (installed automatically)

## Installation
* set the environment variable 'LASER' to the root of the installation, e.g.
  `export LASER="${HOME}/projects/laser"`
* download encoders from Amazon s3 by `bash ./install_models.sh`
* download third party software by `bash ./install_external_tools.sh`
* download the data used in the example tasks (see description for each task)

## Applications

We showcase several applications of multilingual sentence embeddings
with code to reproduce our results (in the directory "tasks").

* [**Cross-lingual document classification**](tasks/mldoc) using the
  [*MLDoc*](https://github.com/facebookresearch/MLDoc) corpus [2,6]
* [**Bitext mining**](tasks/bucc) using the
  [*BUCC*](https://comparable.limsi.fr/bucc2018/bucc2018-task.html) corpus [3,5]
* [**Cross-lingual NLI**](tasks/xnli)
  using the [*XNLI*](https://www.nyu.edu/projects/bowman/xnli/) corpus [4,5,6]
* [**Multilingual similarity search**](tasks/similarity) [1,6]
* [**Sentence embedding of text files**](tasks/embed)
  example how to calculate sentence embeddings for arbitrary text files in any of the supported language.

**For all tasks, we use exactly the same multilingual encoder, without any task specific optimization or fine-tuning.**

## Docker

An image docker has been created to help you with the settings of an environment here are the step to follow :

* Open a command prompt on the root of your LASER project
* Execute the command `docker build --tag=laser .`
* Once the image is built run `docker run -it laser`

A REST server on top of the embed task is under construction, 
to run it you'll have to expose the port 8099 by executing the next line instead of the last command.

* `docker run -p 8099:8099 -it laser`

Once you'll be inside of the docker container, you'll be able to run the Flask server by executing this command

*  `python app.py`

## License

LASER is BSD-licensed, as found in the [`LICENSE`](LICENSE) file in the root directory of this source tree.

## Supported languages

Our model was trained on the following languages:

Afrikaans, Albanian, Amharic, Arabic, Armenian, Aymara, Azerbaijani, Basque, Belarusian, Bengali,
Berber languages, Bosnian, Breton, Bulgarian, Burmese, Catalan, Central/Kadazan Dusun, Central Khmer,
Chavacano, Chinese, Coastal Kadazan, Cornish, Croatian, Czech, Danish, Dutch, Eastern Mari, English,
Esperanto, Estonian, Finnish, French, Galician, Georgian, German, Greek, Hausa, Hebrew, Hindi,
Hungarian, Icelandic, Ido, Indonesian, Interlingua, Interlingue, Irish, Italian, Japanese, Kabyle,
Kazakh, Korean, Kurdish, Latvian, Latin, Lingua Franca Nova, Lithuanian, Low German/Saxon,
Macedonian, Malagasy, Malay, Malayalam, Maldivian (Divehi), Marathi, Norwegian (Bokmål), Occitan,
Persian (Farsi), Polish, Portuguese, Romanian, Russian, Serbian, Sindhi, Sinhala, Slovak, Slovenian,
Somali, Spanish, Swahili, Swedish, Tagalog, Tajik, Tamil, Tatar, Telugu, Thai, Turkish, Uighur,
Ukrainian, Urdu, Uzbek, Vietnamese, Wu Chinese and Yue Chinese.

We have also observed that the model seems to generalize well to other (minority) languages or dialects, e.g.

Asturian, Egyptian Arabic, Faroese, Kashubian, North Moluccan Malay, Nynorsk Norwegian, Piedmontese, Sorbian, Swabian,
Swiss German or Western Frisian.

## References

[1] Holger Schwenk and Matthijs Douze,
    [*Learning Joint Multilingual Sentence Representations with Neural Machine Translation*](https://aclanthology.info/papers/W17-2619/w17-2619),
    ACL workshop on Representation Learning for NLP, 2017

[2] Holger Schwenk and Xian Li,
    [*A Corpus for Multilingual Document Classification in Eight Languages*](http://www.lrec-conf.org/proceedings/lrec2018/pdf/658.pdf),
    LREC, pages 3548-3551, 2018.

[3] Holger Schwenk,
    [*Filtering and Mining Parallel Data in a Joint Multilingual Space*](http://aclweb.org/anthology/P18-2037)
    ACL, July 2018

[4] Alexis Conneau, Guillaume Lample, Ruty Rinott, Adina Williams, Samuel R. Bowman, Holger Schwenk and Veselin Stoyanov,
    [*XNLI: Cross-lingual Sentence Understanding through Inference*](https://aclweb.org/anthology/D18-1269),
    EMNLP, 2018.

[5] Mikel Artetxe and Holger Schwenk,
    [*Margin-based Parallel Corpus Mining with Multilingual Sentence Embeddings*](https://arxiv.org/abs/1811.01136)
    arXiv, 3 Nov 2018.

[6] Mikel Artetxe and Holger Schwenk,
    [*Massively Multilingual Sentence Embeddings for Zero-Shot Cross-Lingual Transfer and Beyond*](https://arxiv.org/abs/1812.10464)
    arXiv, 26 Dec 2018.
