# gazetteer

Generic lexical gazetteer (hereby referred as "lexical gazetteer") is a high throughput annotation system for real-time indexing of unstructred clinical notes. 

## Gazetteer Architecture

The lexical gazetteer utilizes spaCy’s Matcher [1] class along withEntityRuler [2] class to add the terms in gazetteer lexicon to the spaCy en_core_web_sm [3] model. The Matcher instance reads in ED admission notes and returns symptom mentions and the span of text containing each mention. Returned spans are further processed by the spaCypipeline to search for custom entities added by the EntityRuler. Theoutput is then lemmatized to convert the text to its canonical form.The NegEx component of spaCy (negspaCy [4]) is used for negation detection.

Rule-based matching [5] of generic gazetteer lexicon is automated using the following token attributes in spaCy: base form of the word (LEMMA); universal part-of-speech tag (POS); detailed POS tag (TAG); and punctuation (IS PUNCT) [6].

## Data

1. Lexicon of clustered terms: GAZ_group.csv

## Requirements

- pandas==1.1.3
- scispacy==0.3.0
- spacy==2.3.5
- negspacy==0.1.9

## Creating and Executing Gazetteer

To create a docker image, simply run the following in the main directory:

```docker build -t ahc-nlpie-docker.artifactory.umn.edu/covid_gazetteer .```

To run the created docker, type:

```docker run -it  -v /var/lib/docker/data/vte/icd_negative/:/data ahc-nlpie-docker.artifactory.umn.edu/gazetteer:1  python -u  /home/gazetteer/gazetteer_multiprocess_sbd.py final_lex.csv manifest_negative.csv data_in/ negatives vte```

The important arguments to docker command are:

   - notes_to_process.csv: manifest of notes to be annotated. The file should not contain any header columns.
   - data_in: directory containing the notes.
   - mount_dir: directory containing notes_to_process.csv and data_in directory.
   - ann_out: annotated output.
   - prefix_term: phrase to prefix the features in the output.

References:

1. spaCy Matcher: https://spacy.io/api/matcher

2. spaCy EntityRuler: https://spacy.io/api/entityruler

3. spaCy models: https://spacy.io/usage/models

4. negspaCy: https://spacy.io/universe/project/negspacy

5. spaCy Rule-Based Matching: https://spacy.io/usage/rule-based-matching

6. spaCy Token: https://spacy.io/api/token
