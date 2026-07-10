here we begin studying interpretability methods, first, the SAE (sparse auto encoder)  

previously we explained how we view each activation as a combination of features added together. in SAEs we try to decompose those features into interpretable units.  

# SAEs

features = encoder (activation)
pred_activation = decoder (relu (features))

encoder and decoders are linears. 
each decoder column would be a feature, that decomposes the activation, corresponding with a encoder neuron activation.  
decoders has its columns magnitude normed because we want how much of it fires to be only dependent on the activation

there are more activations than dimensions. so any decomposition would reconstruct. so we need some contrainsts, and SAE uses sparsity as the constraint.  
it would be beautiful if the most mathmatically compressive coding is also the most interpretable one. and it make sense, because in order to decompose all activations of the model inferenced on a real dataset, the most compressive one should be to find the features the model actually are using. if the activations are random, sparse decompositions shouldn't be possible, cause we need like 20 features to decompose a 2048 dimensional activation.  

the decoder columns are the features we interpret. each activation should equal to activated featuress * activations + reconstruction losss  

the encoder and decoder columns are not weight tied. we can interpret encoder rows as a solver for the overcomplete basis to ensure sparsity. each individual encoder is "one activation direction where this decoder column feature is useful for decomposition". particularly, it could be aiming for roughly that feature plus several statistically related features.   

the encoder bias is a expansion_dim shaped vector added to the activations  
the decoder bias is a embed_dim shaped vector substracted from the input before the encoder row matmul. the purpose is to find a better center for the encoder row to differentiate from than the origin.  

therefore, multiple encoder rows with similar values make sense, because they can relate to different decoder columns, like saying in this activation direction can be decomposed into these features. and multiple decoder columns with similar values make sense, because one feature can and should be useful in multiple activations detected by different encoder rows.  

we have to decide the expansion factor. it depends on the amount of "useful concepts" the model uses. i have no theory i will just pick 16x due to compute constraints. anthropic can go 10,000x and some earlier research went 16x. since SAEs can only find rough ideas and we're not discovering deep mathmatical circuits yet anyways i think 16x is fine.  

# l1 sparsity loss

because sparsity what we want to enforce, we add a l1 loss on the activations. we hypersearch the sparsity loss and reconstruction loss ratio to keep say less than 100 features firing, and recon more than 0.8 variance explained   

# jumpRelu  

rather than relu at 0 we relu at a trained parameter  
this helps with reducing nonorthagonality cross influence. 
we use jumprelu with the L0 sparsity loss which is a simple count of activating features  


## need to add: how to train it backwards kernel trick with pytorch detach. how to train backwards with l0 loss.    

# topk

an alternative approach to sparsity loss is a top k gate before decoding during training. this forces the model to only use say top 10 activating features.
we will probably use this instead of sparsity because hypersearch on sparsity loss takes too much time and this is cleaner as we can just directly set a number.  
a concern is this kills to many gradients.  

# radialGate

as written in mlp.md outputs depend on nonlinear regions. they are polytopes divided by each neuron's decision boundary. however, standard saes don't consider this. it assumes the activation along the same direction would always produce the same features and scaling it stronger or weaker scales the same features stronger or weaker. but as an activation scales in the same direction, the actual output it can produce in later layers where it's the input could have different neurons start activating. and if everything we interpret is grounded by eventual outputs then surely features exist at some magnitude direction pairing rather than just direction.  

the first idea is to learn an additional centroid and gate the activation by the activation's difference from the centroid.  

this is however, maybe untrainable. because if the centroid drifts apart from used activations range there's no gradients.    

# slabGate

to try to capture the polytope region we modify the jumpRelu to have both a lower gate and a upper gate  
this will create a slab in embedding space, that will disable a encoder row if it's magnitude is too big. standard jumpRelu cannot do this.  
however, a slab cannot constraint the whole space into a enclosed region like a polytope. it also cannot differentiate a directionally similar activation to a less directionally similar but high magnitude activation. however assuming the residue stream or layer_out is about uniform in value or normed this would matter less because no activation would be that high magnitude.  
i guess this is a practical question answerable by actual gemma data on mean and std on activation magnitudes.     

# coneGate

we can add a cone shaped gate which when intercepted with the slab creates an enclosed region  
we cone gate by the cosine similarity between the activation and the encoder row with a learnable "minimum angle" theta.  
this should also help reduce non orthogonality cross influence  
however, the natural matmul is already a sort of angle * activation_magnitude * encoder_magnitude so we are basically multiplying the feature_activation by (1/ (activation_magnitude * encoder_magnitude) which seems redundant  

with all these gates we essentially ask "at what region do we use this decoder feature"  
