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





## Segment-level Recurrence with State Reuse

+  the hidden state sequence computed for the previous segment is fixed and cached to be reused as an extended context when the model processes the next new segment



## Relative Positional Encoding

원래 트랜스포머에서 사용한 positional encoding의 경우 세크먼트 간의 차이를 나타내지 못한다. 예를 들어 각 세그먼트의 첫번째 토큰은 같은 positional encoding과 더해진다. 



+ by injecting 