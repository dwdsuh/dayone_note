# Misspelling Oblivious Word Embedding

- Misspelling triggers a large amount of OOV(out of vocab)
- simply allowing the inclusion of misspellings into corpora and vocab in existing methodiligies might not provide satisfactory results. --> the sparsity of misspelling



## contributions

+ a novel problem and a non-tirvial solution to building word embeddings resistant to misspellings
+ a novel evaluation method specifically siutable for evaluatiing the effectiveness of MOE
+ a dataset of misspellings to train MOE



## MOE

### the fastText

+ skip-gram with negative sampling
+ embed subwords(character n-grams) and use them to construct the final representation of w[i]

### MOE

the scoring function is defined as the dot product between the sum of input vectors of the subwords of w[m] and the input vector of w[e]



## Misspelled data generation

