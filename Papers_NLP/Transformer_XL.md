# Transformer XL: Attentive Language Models Beyond a Fixed-Length Context



+ Transformer have a potential of learning longer-term dependency, but are limited by a fixed-length context in the setting of language modeling. 
+ learning dependency beyond a fixed length without disrupting temporal coherence
+ segment-level recurrence mechanism and a novel positional encoding scheme







+ problem: long-term dependency
+ RNN, LSTM, GRU  => it is difficult to optimze RNN-based model. 
+ Transformer(char-level) faces 
  + seperated fixed-length segments of a few hundred characters, without any information flow across segments
  + context fragmentation
    + no semantic boundary
    + the model lacks necessary contextual information needed to well predict the first few symbols
+ Transformer XL
  + introduced recurrence into a deep self-attention network
  + reuse the hidden states obtained in previous segments



## Contributions

+ the notion of recurrence in a purely self-attentive model 
+ deriving a novel positional encoding scheme.