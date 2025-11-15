# code1-hw4
Rahul kuduru 700763240
import spacy
spaCy provides the tokenizer, POS tagger, lemmatizer, and stopword list used by the pipeline. If spaCy is not installed you'll get an ImportError. Install with pip install spacy.

# Load spaCy model
nlp = spacy.load("en_core_web_sm")
nlp is a callable object that runs the full NLP pipeline (tokenizer, tagger, parser, NER, lemmatizer, etc.) on input text.
Notes / Gotchas:

The model must be downloaded first with python -m spacy download en_core_web_sm. If missing, you’ll get an OSError about not finding the model.

en_core_web_sm is the small model — it’s fast and good for demos but less accurate than the medium/large models. It contains the components needed for tokenization, POS tagging and lemmatization.

text = "John enjoys playing football while Mary loves reading books in the library."
This is the test sentence the pipeline will process. Replace with any other text you want to analyze.

doc = nlp(text)
Running nlp(text) performs tokenization, POS tagging, lemmatization, and other preprocessing steps the model provides. doc is iterable and each element is a Token object with many useful attributes (e.g., .text, .lemma_, .pos_, .is_stop, .is_alpha).

# Allowed POS tags
allowed_pos = {"NOUN", "PROPN", "VERB"}
he pipeline wants to keep only tokens that are nouns or verbs; using a set gives fast membership checks.
Notes: If you want to exclude proper nouns, remove "PROPN". To keep adjectives, add "ADJ".

result = [
    token.lemma_
    for token in doc
    if (not token.is_stop)
    and token.is_alpha
    and token.pos_ in allowed_pos
    
]
This is a compact list comprehension that builds the final list result. 

for token in doc
Iterates token-by-token through the processed doc.

if (not token.is_stop)
Stopwords are usually not useful for many tasks (search, indexing, some ML models) and are filtered out here.
Note: spaCy’s stopword list is language-specific and modifiable. If you want a custom list, you can check/modify nlp.vocab or use your own set.

and token.is_alpha
Filters out punctuation (.), numbers (123), and tokens with mixed characters (e-mail).
Note: If you want to keep hyphenated words or apostrophes, remove or change this check.

and token.pos_ in allowed_pos
Focuses output on nouns and verbs as requested.

token.lemma_
The lemmatized (base) string for the token, e.g., playing → play, books → book.
Lemmas reduce morphological variants to a canonical form. Note that .lemma_ is a string; .lemma is the integer lexeme id in spaCy’s vocab.

Result: result is a Python list of lemmatized tokens that passed all filters.

print(result)
hows the pipeline’s output when you run the script in Colab, terminal, or Actions logs.

Final printed list:
['John', 'enjoy', 'play', 'football', 'Mary', 'love', 'read', 'book', 'library']


