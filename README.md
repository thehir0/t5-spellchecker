# Context-sensitive Spelling Correction

The goal is to implement context-sensitive spelling correction. The input of the code will be a set of text lines and the output will be the same lines with spelling mistakes fixed.

### Solution is to train encoder-decoder transformer for seq2seq spelling correction task

[Norvig's solution](https://norvig.com/spell-correct.html) is quite simple without the use of neural approaches, in contrast to this solution, an idea emerged on how much more effective modern neural approaches are than old probabilistic approaches. 


### The model was trained on two datasets:

1. [Ag_news](https://huggingface.co/datasets/ag_news) 

    AG is a collection of more than 1 million news articles. News articles have been gathered from more than 2000 news sources.

    ### Data Splits

    | name  |train |test|
    |-------|-----:|---:|
    |default|120000|7600|
    
&nbsp;

2. [Google wellformed query](https://huggingface.co/datasets/google_wellformed_query)

    Google's query wellformedness dataset was created by crowdsourcing well-formedness annotations for 25,100 queries from the Paralex corpus

    ### Data Splits

    | name  |train |valid|
    |-------|-----:|---:|
    |default|17500|3750|

### How to create spelling mistake?

The approach is to generate some modifications of word with an optional probability parameter (default: 0.4) which determines the likelihood of applying a modification to the word. The types of modifications(all edit distance 1) include deletion, transposition, replacement, and insertion. The probabilities for each type of modification are equal.

Here's a brief overview of what each modification does:

1. **Deletion**: Randomly deletes one character from the word, unless the word has only one character.
2. **Transposition**: Randomly selects a character in the word (except the first and last characters) and swaps it with its adjacent character.
3. **Replacement**: Randomly selects a character in the word and replaces it with a randomly chosen alphabet character.
4. **Insertion**: Randomly inserts a randomly chosen alphabet character at a random position in the word.

The reason to use only edit 1 is because edit distance 2 can cause huge deviations from the original word and edit distance 1 is the most common type of misspelling.

### Metrics

| Solution              | Recall | Precision |
|-----------------------|--------|-----------|
| Norvig solution       | 0.844  | 0.895     |
| t5-small spellchecker | 0.864  | 0.952     |

### Further improvement

1. Increase the edit distance depending on the length of the word.
2. Train on more data.

### Example

You can try your example using [HuggingFace Inference API](https://huggingface.co/the-hir0/google-t5-small-spellchecker)

  `he askd with a deep vocie -> he asked with a deep voice`
  

