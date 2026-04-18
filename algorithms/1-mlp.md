# mlp  

as said before in features.md, representations can be any concepts and transformations is the mapping between them.  
activations form representations, and weights are the learnt transformations. they co-define each other numerically (an activation is only defined on the later encoder layer and a weight is only defined on what it's output means. hence we need something external such as inputs and outputs to ground the interpretation). in this article we try to think about how to interpret weights generally and then for the MLP layer specifically  
neurons, both biological and silicon, uses two fundemental operations: matmuls and relus. (note that biological neurons have binary firing with a temporal rate while silicon neurons use an activation value to approximate rates). i will try to explain why these two operations are so fundemental, at a both numerical and conceptual level.  

# matmuls  

each individual neuron with n weights and 1 bias is essentially a nd -> 1d mapping. and a layer of neurons with m neurons  would be a nd -> md linear mapping.  
for a single neuron it is a nd hyperplane with some bias shift. (nd hyperplane through n+1d space because we include output as a dimension)  
for visual intution picture the 1d->1d mapping. a neuron say x is its input y is ts output, would be a straight line. for 2d->1d mapping, where a neuron's input is xy and output is z, it would look like a plane.  

the conceptual understanding is through features. we always think of features as starting from the origin. the hyperplane is essentially a feature detector where the feature is the neuron's weights. (say a neuron's weights is 2x + 3y it's feature would be [2,3]) and the activation would be how much the input went along the feature direction plus the bias (high bias shifts plane upwards causing inherently more activation, negative bias vice versa)  

## TO WRITE! IMPORTANT!

matrix level intuition. rank. rotate, streth, shear. neurons as basis. why 2d->3d projection must live in a 2d hyperplane. what's the point of sclaing up and down dimensions.  
 but this is a different view  tho you arent viewing EACH NEURON as a feature anymore you are viewing the nth coordinate of each neuron as a feature

some theories:  
low ->  high projection: neuron view.  
high -> low projection: column view.  
activation with relus after: neuron view.   
works for both superposition compression, and mlps, and saes.  
reasons:  
low -> high means there's more neurons than input_dim of activations, so each neuron should detect features in embedding.  
high -> low means there's less neurons than input_dim of features, so each feature should scale  a column
relus means nonlinearity happens at the neuron level which forces a neuron level information encoding

actually i now renounce this view. it can be a high -> low feature detection for example maybe attention QK.  

honestlyt the distinction is not dimensionality i  was wrong the distinction is what  we interpret them as wanting to accomplish whether they  want to accomplish extracting features or using features

you want to relu post feature output, because that prevents features in opposite  directions from outputing something negative. and you do not want to relu post activation output because that kills the superposition.  

matmuls as scaling a space. (dragging an axis one point fixed at origin other point wherever whole space follows). down projection as squashing up projection as lifting. whatever svd is)  

i feel like im actually pretty fucking close to uniting the two views ngl. the neuron feature view. is low to high projection. you are. lets justpicture 2d->3d ok. you are putting some low dimension activation onto 3d space. that activation is a single number but you decompose it say into here 3 features. then using the drag view you drag each of those three to align to some axis. thats the fucking matmul. and you get from activations to features. reverse view. features to activations. you start with the three axis aligned basis. you squash at some angle. then drag them to superposition.

even if the neuron level view doesn't work it is still natural to try it before doing SAEs.  

(maybe break this into two section one after relu)

# relus  

without relus, any layers of neurons could just have their weights be multipled together into one layer, making depth meaningless.  
for visual intution picture the 1d->1d mapping. without relus, all neurons in layer 1, would be a straight line. and layer 2 would have ways to scale and shift these straight lines. but no matter how it scales and shifts, when added together it's still a straight line. not matter how many layers it would always just be a straight line. but say there is a relu after each layer. after first layer and relu, each neuron's effective output would be a line with a inflection point. and at layer 2 you can combine these differently  shaped lines together into more jagged line. and at layer  3, you have layer 2's jagged lines as input to scale and shift, creating even more jagged lines. until eventually you can approximate any function.  
this is why only matmul and relus are needed, they can approximate any functions wihout needing inputs to multiply each other or have a non one power.  
for more visual intuition picture a 2d->1d mapping, each neuron without relu would be a plane. and at layer 2 no matter how much you scale and shift the planes and add them together it would still be a single plane. but with relu you can create a jagged mountain like shape. and at future layer even more jagged and descriptive shapes.  

during training, the model have the inputs and outputs, and it tries to form the shapes it can to reconstruct the output from the input. 

## TO WRITE!  
clean this up

oh fuck it RUINS THE ENTIRE FUCKING SUM i forgot EACH NEURON IS A SUM like say the next  is a square detector that has like 4 angles   it should  be a square but the fucking  wait no it literally doesnt because then why not just have a negative value  for curves . but i guess even like maybe there is like OH FUCK THEN if there is NO ANGLES JUST CURVES then like then IT  WOULD SAY A CIRCLE IS A SQUARE CLAUDY IT WOULD SAY A CIRCLE IS A SQUARE.
But wait why not just set the curve weights to 0. 
Ask Claude.
Oh it’s because say it’s a square with rounded corners you want square to fire less strongly you WANT NEGATIVE WEIGHTS FOR CURVES. But you DON’T WANT negative curves coming in  to be positive. 
Wait but what’s wrong with negative weights. They won’t cause circle fire square. positive  weight  would make circle fire square. a negative weight wouldnt.  so what is the problem with negative weight on curves why does it still need relus?  
is it cause maybe we  need a third class right say triangle slangted angels then its still not circle but then we would predict square because curves fire negatively with negative weights but its literally not a square dykwim. i wrote this many weeks before does this have any point to you at all
note that not all mlp layers decoder ALL features. just SOME. that it will use to add information. the rest get preserved in the skip connection. 
 Each row of M is a direction in input space with a magnitude. that is the "default" input-independent scaling of that output feature.  
each column of M is a direction in output space with a magnitude. that is the "default" input-independent sclaing of what a feature adds to the output space  






note that i titled this relu but it applies to other similar activation functions like gelus.  
one particular case is the SwigLU. which 1.5x the MLP size by using a encoder double the size, one for gating one for information. this is because different coordinates can be used for gating than information.  



# mlp neuron level  


MLP is kind of structurally similar to a SAE: expanding the embed_dim to a expansion for 4x, relu, then projecting it back done. combined with the fact that relus cause a priviledged basis that makes superposition less likely, it is worthwhile to interpret the MLP hidden layer neuron level, in addition to SAE and WCCs.  

this is a good place to actually test the theory of neurons as individual feature detectors. not just with interpretation  and pertubation. but also just by seeing how orthogonal the encoding rows are and whether they follow a roughly superposition structure (not a complete structure cause not all features is detected in a mlp) and also how superposition structure the outputs are. note that there could be repeating features or just general close ones making this harder to determine. but overall there should be some "superposition score" to evaluate  the superposition hypothesis.     

interpretation wise check activation monosemanticity based on context. auto categorize auto determine throguh llm api calls.  

we could also use SAE or WCC features and try to see if any MLP encoding neurons match them. 

# feature tables  
note this is an original idea in developement not anthropic's and not yet tested  

interpret what MLP does by passing input activation feature combinations through and seeing output feature combinations   

assuming activation interpretability is somewhat solved with WCC   
we simply pass the decoded input features in to the original MLP (do not change it in anyway, keep the nonlinearity) and get the output, decompose the output into features what the output layer's WCC, and see the top k activation values of output features  
we can then try different combinations of input features, like activating 2 or 3 input features and add their decoded activation and see the output  
we can also see if different ratios, like activation value 2 for feature A and 1 for feature B, and how that changes the output feature activations  
note that here we count on WCC to remove any repeated features in the residue stream to their respective original previous layers, otherwise most mappings would just be amplification of the same idea.  

bias pertubation experiment:  
to add some idea into the MLP model, traditionally researchers would add an external SAE decoding into the residue stream. we can do that, but we can also make it permanently part of the model by just adding the combination we want to the bias term of the MLP decoding.  

weight pertubation experiment:  
viewing MLP as a feature mapping table, we will modify the weights to change one specific input feature output feature relationship, say from Apple -> red to Apple -> blue, and see if that changes what the model outputs.  
this is very cool because it should work across prompts. and it actually changes the model rather than for example an external SAE attached to the model  

we might have to change multiple relationships to get an effect. for example first zero out all the apple -> red related features then turn up all the apple -> blue  related features. or maybe it goes (apple -> color) in one layer then (color ->red in a later layer) 
we would also have to target all mlp across layers that stores this relationship not just a single MLP. this is a wrong assumption attention V or O doesn't store information. but hopefully MLP effects can dominate enough to change outputs.  

to actually change the weights, we have to retrain that specific MLP layer. we need to preserve as much as possible of all other relationships and try to specifically target the relationships we want to change. note that the data would be float activation values (i.e. 1.1,1.2,1.3 ... not uniform i.e. 0,1,2 ...)  

here we keep the bias unchanged to avoid intefering with the original feature mappings too much and in case we want to run both weight and bias changes together.  

