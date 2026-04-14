# Notes for the paper In-context Learning and Induction Heads

https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html  

implementation in svnapse/probes  

induction heads does in context learning. A* B* -> A B. In some meaning space.  
the first layer head at token B* copies A* into B* 's activation. the second layer head at A queries for A* and copies B* over.  

we can understand circuit functions by seeing their forming process during training and the training loss phase change decrease. Not just reading and editing activations.  

few shot learning: providing examples in prompts help model learn tasks without weight change  

in context learning score: the loss difference between the 500th token and the 50th token (arbitiarily chosen numbers)  

the first head (i will call it the copy head but the paper didn't give it a name) copies the previous token into current token. this fires on all tokens? to allow later induction. so A B, copy head copies A into B's activation.  
induction head in a later layer looks for this copied head part and makes it more likely to output. so A2 attends to B's copied A part and makes it more likely to output B.  

copy head and induction pair exist in pairs?
copy head Q at B will attend to copy head K at A maybe through positional embedding or by meaning. the OV of copy head at A will compress A into subspace1. which will receive a high attention score. let's say B is encoded in subspace2. 
so B's activation space in the post attention is subpsace1 of compressed A and subspace2 of  compressed B. 
at some later layer induction head:
induction head Q at A2 will look for similar A. induction head K at B will extract some meaning of A from the two subspaces so maybe it just primarily looks at subspace1. This results in high attention score. the OV of induction head at B will copy the B, maybe primarily looking at subspace2. This will result in B's compressed activation being copied into A2's post attention activation.  

eigenvalues can be used to detect the OV copying by seeing the sum of eigenvalues over the abs sum of eigenvalues so more positive this number is is a heuristic for the higher like direct copying percentage. note that this doesn't consider how much differnt eigenvectors make up the activations, like maybe some are more important than the others?  

note that A and B can relate to A* and B*, which are roughly similar representations in more complex examples that are not the exact same token but maybe of a similar meaning. this makes it much more general.  
also note that each copy and induction step can be based on multiple tokens or really just a complex meaning rather than just two.  

so what is the general insight really. past the point of just a small toy induction head, how does this generalize to complex circuits. well the copying head basically combine multiple token representations into one through compression and concatenation. which could later be used for like a silu basically using some part to gate and using some parts for value. and then the induction head is basically well the looking pair decides what is the gate and the other providing pair decides what value to give. like a silu but cross token positions and with subspaces rather than coordinates.  


(I DID NOT READ THE REST OF THE PAPER YET)

