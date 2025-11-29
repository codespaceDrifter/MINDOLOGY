# notes for transcoder paper research
will implement on a small model later in torchrvn repo interpretability folder

## on the biology of llm paper notes
https://transformer-circuits.pub/2025/attribution-graphs/biology.html

## cross layer transcoder

(NOTE i think the algo i described is wrong i need to read the circuit tracing paper to figure it out)

a single module that replaces all of the original ffw modules in a transformer. 
weights: a FFW module of dimension [embed_dim] --(linear)-> [features] (features is much larger than embed_dim) --(relu)-> --(linear) -> [embed_dim]
training data: the original ffw inputs and outputs across all layers
training loss: MSE on original ffw data + L1 regularization  
($\lambda * \sum_i |z_i|$)  
$z_i$ being the result AFTER RELU. this encourages sparsity. note that it is NOT the final result after the second linear.
note: the transcoder is a SINGLE set of weights on a SINGLE transcoder module trained on EVERY ffw modules' data rather than having the same number of transcoders as ffws.
and then features are labelled automatically by seeing which features are most active on which contexts. maybe loop through context, create a feature -> contexts (small context window perhaps not full sentencce) where the feature is activated map. and then auto summarize.

## specific findings

different features activation across levels form reasoning chains. you can test the effect of each by turning it up (times a positive number) or turning it down (times a negative number)
the relationship between features can be if chainging the previous feature affects future features
similar features can be grouped together as feature cluster for cleaner graphs
<div align="center"><img src="./imgs/graph1.png" width="500"/></div>

#### future plans affect intermediate word
<div align="center"><img src="./imgs/graph2.png" width="500"/></div>

we can see the model plans to say 'rabbit' on a feature of the end of line token so the model says 'like' to prepare for a rabbit analogy

#### multipligual circuits

<div align="center"><img src="./imgs/graph3.png" width="500"/></div>
different languages share the same features. with an additional language feature determining output language.
<div align="center"><img src="./imgs/graph4.png" width="500"/></div>
the features can be tested by switching operands, operations, of languages

#### addition
<div align="center"><img src="./imgs/graph5.png" width="500"/></div>
<div align="center"><img src="./imgs/graph6.png" width="500"/></div>

rather than having a clean computerized / formal addition logic, models seems to rely on many circuits. including last digit circuits, rough estimate circuits, and special modulo result detection circuits. however when asked to self reflect the model says it did the formal logic.

#### halucinations
<div align="center"><img src="./imgs/graph7.png" width="500"/></div>
due to post training, models have a default answer of i don't know in obscure questions. when seeing a known entity it inhibits the i dont know feature.
