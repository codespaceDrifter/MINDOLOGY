# features  

to understand activations, there are two options. first, we look at specific neurons. second, we look at it as a whole. due to population coding and superposition we will understand it as a whole.   
we try to break the entire activation into a combination of features. features should be the understandable concepts that form a activation   

but why are there features at all? thinking about the mindology theory, one purpose of the mind is efficient world simulation for action sequence planning. and i would argue, different minds, silicon or biological, would converge  to the same set of features when modeling the same reality.   
not saying that all transformer features will be interpretable. some of it could just be data folding purely geometrical. some of it could be concepts of subject matters i do not understand. but for understanding mech interp i think the features that both me and the model understands are enough to build theory.  


# superposition

the model will need much more features than neurons, due to the world being very complicated needing millions of features but it only having an embed_dim of perhaps a couple thousand.  
therefore rather than using each neuron as an orthogonal basis, it  will use the entire layer to form somewhat orthogonal basis. we can measure "cross influence" by assuming the later layer encoding is the same as that feature (it could be encoding of a specific combination of features too). then say feature A fires, how much activation will the later layer encoding for feature B detect, is how much it cross influences.  

# direction coding

the model could learn to utilize feature cross influence as much as possible in their token embedding and also later features. for example say two features are closely related, they could be placed near each other so A causes some of B to fire. if two features express the opposite idea, they could be placed opposite to each other so A causes B to read negative values. if  two features are unrelated, they should be placed orthogonally so A has no effect on B.  

# linear representation hypothesis

we hypothesize that adding together scaled features represent a concept that is their comibnation. this is because the neural network  naturally uses a lot of addition (matmuls, residue streams, training gradients).   

# SAE

we try to decompose features with sparse autoencoders. training a (embed_dim, expansion * embed_dim) encoder, relu, weight tied transposed decoder, with sparsity loss, to break down the activation collected over some dataset into sparse features.  

we can interpret those features with the context that caused  them to  fire, their direct decoding in word embedding space, and pertubation experiments. note that feature decodings should also form a sort of coherent linear representation relationship like word embeddings.  

we choose to train on pre-norm residue stream at each layer. beacuse that is the stuff each layer actually adds to. so if we want to run pertubations it would be easier. (although there is an argument for post norm because that is what each layer actually sees).  


# WCC

one problem with SAEs is residue stream's skip connections mean that the same features is repeated a lot of times in subsequent layers. therefore it would both make later generating refined graphs very difficult and make each activation composed of many features that might be harder for later layers to sparsely decompose. so we use WCCs (weakly causal crosscoders) instead.  
WCCs at each layer encoders that residue stream, then has seperate decoders for that and all subsequent residue stream activations. we train all WCCs jointly, adding all their decodings together to predict a layer's activation.  
the idea for using individual decoders is that the encoder will capture a feature, but that feature's representation would change as the layer's progresses (since each matmul through a weight essentially completely changes the basis), so each decoding should learn the feature the encoder encoded's representation in their respective layers. however this could pose a problem where it drifts into related ideas instead rather than the same idea, say WCC at layer 3 encoded feature "apple" and it turns out "apple" in the training data happens a lot with "red" so some later layer decoding decodes into "red" instead. we would hope this unintended consequence does not happen because hopefully training leads some later WCC to encode "red" and decode "red" to it instead.  

# clustering features  

we should run hyperparameter search on expansion factor and sparsity loss, aiming for about an average of 50 of features firing above a certain threshhold like 0.1 per activation while having less than say 25% dead features what fire say less than 5% of inputs above 0.  
but even after this, a lot of features could have very similar meanings. we want to cluster them together. there's two ways to determine clusters: either semantically by their interpretation, or numerically by their encoding cosine similarity. and after deciding we have another choice: either to just merge them together in the frontend generated visualization tool, or to literally merge their weights somehow (only works for numerical similarity). i think for now i will choose to cluster based on interpreted meaning and only merge them in the frontend.  

