## NMT of Rare Words with Subword Units

[link](https://arxiv.org/pdf/1508.07909.pdf)

### BPE

+ A simple data compression technique that iteratively replaces the most frequent pair of bytes in a sequence with a single, unused byte
+ In the paper, instead of merging frequent pairs of bytes, they merge characters or character sequences.
+ For efficiency, we do not consider pairs that cross word boundaries. 

```python
import re, collections

def get_stats(vocab):
  pairs = collections.defaultdict(int)
  for word, freq in vocab.items():
    symbols = word.split()
    for i in range(len(symbol)-1):
      pairs[symbols[i], sybols[i+1]] += freq
  return pairs

def merge_vocab(pair, v_in):
  v_out = {}
  bigram = re.escape(" ".join(pair))
  p = re.compile(r'(?<!\S)' + bigram + r'(?!\S)')
  for word in v_in:
    w_out = p.sub("".join(pair), word)
    v_out[w_out] = v_in[word]
  return v_out

vocab = {"sample1": 4, "sample2" : 5}

num_merges = 10
for i in range(num_merges):
  pairs = get_stats(vocab)
  best = max(pairs, key = pairs.get)
  vocab = merge_vocab(best, vocab)
  print(best)
```





#Subword Regularization: Improving NMT models with Multiple Subword Candidates 

[link](https://arxiv.org/pdf/1804.10959.pdf)

### Unigram LM

+ Method

1. Heuristically make a reasonably big seed vocab from the training corpus
2. Repeat the following steps until vocab_size reaches a desired vocab size.
   1. Fixing the set of vocab, optimize p(x) with the EM algorithm
   2. Compute the ***loss***[i] for each subword ***x***[i] where ***loss***[i] represents how likely the likelihood is reduced when the subword ***x***[i] is removed from the current vocabulary
   3. Sort the symbols by ***loss***[i] and keep top k% of subwords. Note that we always keep the subwords consisting of a single character to avoid oov. 

+ subword segmentation with the unigram LM can be seen as a probabilistic mixture of characters, subwords and word segmentations. 



### BPE vs. Unigram LM

+ BPE is a variant of dictionary substitution encoder that incrementally finds a set of symbols such that the total number of symbols for encoding the text is minimized. 
+ Unigram language model is reformulated as an entropy encoder that minimizes the total code length for the text. 
+ Both share the same idea that they encode a text using fewer bits with a certain data compression principle(dictionary vs. entropy). 
+ the Unigram LM is more flexible as it is based on a probabilistic language model and can output multiple segmentations with their probabilities, which is an essential requirement for subword regularization. 



### Subword Regularization

+ Subword regularization: 
  + samples one subword segmentation from the distribution P(x|X) for each parameter update. 
+ Previous work
  + dropout: randomly turns off a subset of hidden units during training. 
  + Denoising Auto-Encoders, where noise is added to the inputs and the model is trained to reconstruct the original inputs. 
  + randomly alter the word order of the input sentence and the model is trained to reconstruct the original sentence. 
  + Word dropout: the embedding of a certain word sequence is simply calculated by averaging the word embeddings. 
+ Limitation of previous approaches
  + these previous approaches often depend on heuristics to generate synthetic noises 
  + these appoaches can only be applied to source sentences, as they irreversibly rewrite the surface of sentences. 
+ Subword regularization
  + generates synthetic subword sequences with an underlying language model to better emulate the noises and segmentation errors. 
  + data augmentation: an input sentence is converted into multiple invariant sequences, which is similar to the data augmentation for image classification tasks, fandom flipping, distorting or cropping. 



## Sentencepiece

[link](https://github.com/google/sentencepiece)

+ supports BPE, Unigram Language Model
+ Characteristics
  1. the number of unique tokens is predetermined
  2. Trains from raw sentences
  3. Whitespace is treated as a basic symbol
  4. Subword regularization







# BPE-Dropout: Simple and Effective Subword Regularization

+ drawback of BPE
  + deterministic nature
    + it splits words into unique subword sequences, which means that for each word a model observes only one segmentation.
    + a model is likely not to reach its full potential in exploiting morphology, learning the compositionality of words and being robust to segmentation errors
    + subwords into which rare words are segmented end up poorly understood

+ our approach: produce multiple segmentations of the same word using BPE
  + use vocab and merge table built by BPE
  + at merge step, some merges are randomly dropped. 



## Contribution

+ introduce BPE-dropout
+ BPE-dropout outperforms both BPE and previous subword regularization on a wide range of translation tasks
+ analyze how training with BPE-dropout affects a model and show that is leads to a better quality of learned token embeddings and to a model being more robust to noisy input. 



## BPE-Dropout

+ innate ability of BPE to be stochastic
+ During segmentation, at each merge step some merges are randomly dropped with the probability p. 

```python
current_split = BpeSplit(input_string)
merges = current_split.all_possible_merges_of_tokens()
merges = merges.randomly_drop(probability = p)

if merges != []:
  current_split = current_split.apply_merge(merges)

return current_split
```

