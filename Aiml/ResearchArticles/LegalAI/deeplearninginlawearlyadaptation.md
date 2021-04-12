# Deep learning in law: early adaptation and legal word embeddings trained on large corpora

## Tags

:reviewpaper:

## Text classification

### O'Neill sentential modality classification in legal language

* used annotated sentences
* several models including variants of rnns and lstms.
* found bilstms best while models like svm and random forests did not do too well.

### Chalkidis Obligation and prohibition extraction using hierarchial RNNs

* Extracting obligations and prohibirons from service agreements.
* Sentence level annotation.
* Used bilstms, bilstm with fully connected, bilstms used to develop context aware embeddings, bilstms with self-attention, hierarchial bilstms that were sentence level context aware.
* hierarchial performed best with self attention

## Information Extraction

### Nguyen Recurrent neural network-based models for recognizing requisite and effectuation parts in legal texts

* Used Japanese National Pension Law RRE dataset and Japanese Civil Code RRE dataset with annotated sentences as requisite and effectuation and exceptiom parts.
* Trained used single bilstm three layer bilstm model
