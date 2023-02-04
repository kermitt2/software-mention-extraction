This is a forked version of CZI Software mention extraction and linking from scientific articles tool, with some modifications for updated dependencies, fixing some data preparation issues, updating cross-evaluation and adding an evaluation on real mention distribution.

- addition of a `requirement.txt` file to clarify dependencies

- some import modifications to fit the updated packages

- fixing two data preparation issues, see https://github.com/chanzuckerberg/software-mention-extraction/issues/1 and https://github.com/chanzuckerberg/software-mention-extraction/issues/2

- updated cross-evaluation with the above fixes:

```
F1 score: 0.853408
           precision    recall  f1-score   support

 software     0.8169    0.8614    0.8386       404
  version     0.8934    0.9160    0.9046       119

micro avg     0.8339    0.8738    0.8534       523
macro avg     0.8343    0.8738    0.8536       523
```

- addition of an evaluation on an holdout set of articles (20% of the Softcite dataset) with full article content to evaluate the extraction on real mention distribution (the Softcite corpus includes only paragraphs with at least one mention) - this evaluation includes the above fixes too:

```
F1 score: 0.562856
           precision    recall  f1-score   support

  version     0.4547    0.9310    0.6110       232
 software     0.4121    0.8089    0.5460       649

micro avg     0.4229    0.8411    0.5629       881
macro avg     0.4233    0.8411    0.5631       881
```


# Software mention extraction and linking from scientific articles :floppy_disk:

Most of the cutting-edge science is built on scientific software, which makes scientific software often as important as traditional scholarly literature. Despite that, the software is not always treated as such, especially when it comes to funding, credit, and citations. Moreover, with the ever-growing number of open-source software tools, it is impossible for many researchers to track tools, databases, and methods in a specific field. 

In an effort to automate the process of crediting and identifying relevant and essential software in the biomedical domain, we've developed a machine learning model to extract mentions of software from scientific articles. The input to this model is a text from a scientific article and the output is a list of mentioned software within it. 

We applied this model to the CORD-19 full-text articles and stored the output in[CORD-19 Software Mentions
](https://zenodo.org/record/4582776#.YxJZvuzMJQ0). **Cite as:** Wade, Alex D., & Williams, Ivana. (2021). CORD-19 Software Mentions [Data set]. https://doi.org/10.5061/dryad.vmcvdncs0

## Getting started

### Dependencies
    Python 3.7+ (tested on 3.7.4)
    Python packages: pandas, numpy, keras, torch, nltk, sklearn, transformers, os, seqeval, json, time, tqdm, argparse, blink, bs4, re, itertools

----------------------------------

### Training
#### Data

  __Softcite:__ 
  
  * [Softcite data repository](https://github.com/howisonlab/softcite-dataset)
	
  * [Full corpus](https://github.com/howisonlab/softcite-dataset/blob/master/data/corpus/softcite_corpus-full.tei.xml) (downloaded on February 8, 2021)
        
  * Download the XML file above and place it in the ./data folder. 
   
  * XML file processing notebook: ` ./notebooks/Parse softcite data.ipynb`
  	  
	  * *Input:* ./data/softcite_corpus-full.tei.xml
    	  
	  * *Output:* ./data/labeled_dfs_all.csv

----------------------------------

#### Model

  * Model training notebook: `./notebooks/Train software mentions model.ipynb` 
  
	  * *Input:* ./data/labeled_dfs_all.csv, ‘allenai/scibert_scivocab_cased’ [SciBERT: A Pretrained Language Model for Scientific Text](https://aclanthology.org/D19-1371) (Beltagy et al., EMNLP 2019)

	  * *Output:* ./models/scibert_software_sent 
  
  __Performance:__
 
  <img src="https://github.com/chanzuckerberg/cord19-software-mentions/blob/main/img/img1.png" width="600">

----------------------------------

### Inference 

#### Software Mentions
    
  * Download pretrained model 'scibert_software_sent' from: s3://meta-prod-ds-storage/software_mentions_extraction/models and place it in the ./models/ folder. 
  
  * Example of how to run the model in inference mode: `./scripts/Software mentions inference mode.ipynb`
  
  * *Example:* 
  
  <img src="https://github.com/chanzuckerberg/cord19-software-mentions/blob/main/img/img2.png" height="200">

#### Wikipedia Linking 

  * This model is based on [BLINK](https://github.com/facebookresearch/BLINK)
  
  * Follow instructions on the github repo to download relevant models/install. 

  * Example of how to run the model in inference mode: `./scripts/Link text to wikipedia.ipynb`
	
  * *Example:* 
        
  ![](https://github.com/chanzuckerberg/cord19-software-mentions/blob/main/img/img3.png?raw=true)

----------------------------------

### Extract mentions of software from the CORD-19 dataset :microbe: :books:

  * __CORD-19 data__: More information and download instructions: [here](https://github.com/allenai/cord19)
  * Save to the ./data/ folder
  * Run notebook: `./scripts/Software mentions CORD19.ipynb`

----------------------------------
  
### Contributing
Contributions and ideas are welcome! Please see our contributing guide and don't hesitate to open an issue or send a pull request to improve the functionality of this gem.

### Code of Conduct

This project adheres to the Contributor Covenant [code of conduct](https://github.com/chanzuckerberg/.github/blob/master/CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to [opensource@chanzuckerberg.com](mailto:opensource@chanzuckerberg.com).

### License

[MIT](https://github.com/chanzuckerberg/sorbet-rails/blob/master/LICENSE)
