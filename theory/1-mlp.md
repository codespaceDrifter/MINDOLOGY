# mlp

as said before in features.md, representations can be any concepts and transformations is the mapping between them.
activations form representations, and weights are the learnt transformations. they co-define each other numerically (an activation is only defined on the later encoder layer and a weight is only defined on what its output means. hence we need something external such as inputs and outputs to ground the interpretation). in this article we try to think about how to interpret weights generally and then for the MLP layer specifically.
neurons, both biological and silicon, use two fundamental operations: matmuls and relus. (biological neurons have binary firing with a temporal rate; silicon neurons use an activation value to approximate rates).

# matmul

each individual neuron with n weights and 1 bias is essentially a nd → 1d mapping. a layer of m neurons is a nd → md linear mapping.

## row neuron view

each row of W is a direction in input space with a magnitude. that is the "default" input-independent scaling of that output feature. break each weight vector into a unit vector in that direction times its magnitude. then each output coordinate equals the input projected onto the unit vector's length, times the weight vector magnitude, plus bias.

## column basis view

each column of W is a direction in output space with a magnitude. that is the "default" input-independent scaling of what a feature adds to the output space. in the column decoder view it's a 1d → nd line, not a plane.

## hyperplane view

for a single neuron it is a nd hyperplane through n+1d space (we include the output as a dimension) with some bias shift. for visual intuition: the 1d → 1d mapping is a straight line; the 2d → 1d mapping (input xy, output z) is a plane. each feature unit vector is the direction of the hyperplane, the neuron magnitude is the scale, and the bias is the shift.

## space shift view

matmul as scaling a space — dragging an axis with one point fixed at origin and the other point wherever, and the whole space follows. down projection as squashing, up projection as lifting. a good visual: a unit circle of directions at the input becomes an oval at the output. up projection is left-invertible: x = (W^T W)^-1 y for y = Wx.

## dimensionality and rank

a composite is constrained by its rank (the effective output dimension), bounded by the hidden dim. e.g. an (n,1)(1,n) composite is a rank-1 (n,n) matrix.

## any decomposition is valid (gauge)

it isn't just "not unique" — it is gauge. without relu between encoder and decoder, you can rotate them freely and the composite is the same. matmul is the collection of all row-column decompositions.

if we pick either an encoder or a decoder, we can always solve for a valid other (via pseudoinverse) such that the composite returns the same result. neuron row view: each coord (x,y) of the composite is all the input features dot all the output features at the x encoder coord of input and y decoder coord of output. it is scaled from the x coordinate of the input written to y coordinate of the output.

dragging an encoder row around with the composite frozen makes decoder columns NOT related to that row change to a large extent — even reversing signs. so a specific feature-encode → feature-decode mapping in a linear space is meaningless.

composite matrix: constrained by rank (effective output dimension) by their hidden dim. e.g. an (n,1)(1,n) composite is a rank-1 (n,n) matrix.


# polyneuron and polysemanticity

from the neuron view each neuron is polysemantic. from the feature view each feature is polyneuron. the naive "row's feature → column's feature" mapping is wrong: freeze and drag breaks it completely. different encoders in opposite or similar directions can mess up a feature-row → feature-column mapping. each neuron row/column basis feature pairing is ONE real impact among THOUSANDS of neurons. it has to be considered as a whole.

even with relu: relu eliminates the impact of encoders pointing the opposite direction, but it does not eliminate encoders in similar or roughly cross-influential directions with completely different decodings. so MLPs are still polysemantic.

orthogonal = monosemantic = decomposition. overcomplete = polysemantic = projection. (to an extent — not strictly equal.) both relu and sparsity push toward monosemantic.


# relu (and other nonlinearities)

without relus, layers of neurons could just have their weights multiplied together into one layer, making depth meaningless. visualize the 1d → 1d mapping: without relus, every neuron in layer 1 is a straight line; layer 2 scales and shifts these and adds them, but the result is still a straight line, no matter the depth. with a relu after each layer, each neuron's effective output gets an inflection point. layer 2 combines hinged lines into a more jagged line; later layers chain this into arbitrarily complex piecewise-linear functions. for 2d → 1d: each neuron without relu is a plane; with relu you get a mountain shape; later layers bend these into more descriptive shapes.

this is why only matmul + relu is needed — they can approximate any function without inputs multiplying each other or non-1 powers. "locally linear" is a good term — it applies to silu, gelu, etc.

relu is a "voting gate" at each neuron row: other features can turn things on or off for this feature differently than if this feature were passed through alone. if we assume relu is frozen (active set held constant), the feature is still linear, even with negative values going through "on" gates.

note that not all MLP layers decode ALL features — they only decode SOME that the layer uses to add information. the rest get preserved in the skip connection.

layernorm scales each component the same way, so it's still linear.

SwiGLU: 1.5x the MLP size by using an encoder double the size — one half for gating, one half for information. different coordinates are used for gating than for information.

# space carving

different neurons with relus have different decision boundaries, and these boundaries carve the activation space into regions, where within those regions, scaling of a single feature result in a linear scaling of it's output. however past a region, different neurons activate or deactivate, cause a different line upon which the output scales with the input  
this means we shouldn't treat directions as the lowest level of analysis but a direction and magnitude range as the lowest level of analysis.  

# layer is the atomic unit of interpretation

all meaning lives in representations (activations). weights are just polysemantic space shifting — transformations between representations. neuron view doesn't matter, column view doesn't matter; due to polysemanticity they are merely one suggestion in a huge layer. you can only consider the layer as a whole. the layer IS the lowest level of interpretation, and what it does, at that level, is just space shifting.

# linear representation hypothesis

matmuls are linear at the direction level. since scaling input in the same direction only scales output in the same direction (with linear, or locally within regions of non linearity explained later).  

each transformation can be viewed as a sum of: individual encoder alignment × that neuron's weight scale × that neuron's decoder column scaling. matmul doesn't force features to interact — only relu does. you can view the composite vector as a parallel stream of information that only gets combined at relu boundaries.  

this means rather than viewing an activation as one combined feature, we can view it as parrallel features that coexist, interacting only at nonlinearity where they vote on neuron gates.  

this is also supported by the residue stream, where each layer adds something to the original stream.  


# mlp neuron level

an MLP is structurally similar to an SAE: expanding embed_dim by some factor (e.g. 4x), relu, then projecting back down. combined with the fact that relus cause a privileged basis that makes superposition less likely, it is worthwhile to interpret the MLP hidden layer at the neuron level, in addition to SAE and WCC interpretations.

this is a good place to actually test the theory of neurons as individual feature detectors. not just with interpretation and perturbation — also by measuring how orthogonal the encoding rows are and whether they follow a roughly superposition structure (not necessarily a complete one, since not all features are detected in a single MLP), and how superposition-like the outputs are. there could be repeating features or just generally close ones making this harder to determine, but overall there should be some "superposition score" to evaluate the superposition hypothesis.

interpretation-wise, check activation monosemanticity based on context. auto-categorize and auto-determine through LLM api calls.

we can also use SAE or WCC features and try to see if any MLP encoding neurons match them.

# sparse decomposition  

the total thread here is. we want to interpret neural computation into interpretable features. the per neuron view is false due to polysemanticity. so we look to activations. activations as a whole is false because local linearity. so we want to find decompositions of activations that are interpretable. 

to enumerate directions and magnitude bands would be astronomically undoable so we will have to learn it somehow. we have to learn ideally, what the layer actually uses. just assuming we choose a encoder decoder with relu in between. we wnt decoders to be the correct features, and encoders to detect their correct scaling. in a overcomplete decoder basis, any embed_dim number of decoders will perfectly reconstruct the activation. to force SAE to learn the actual features used, we train with a heavy sparsity loss. say it can only activate 10 features for an activation. now on an individual activation  level it is forced to find patterns that actually when added together reconstructs better and on a total data level it would need to learn features that actually get used by the model.  
note that there is an assumption that activations are actually composed of sparse feature combinations we are trying to recover
we can think of decoders as the basis feature and encoder as the "solver for whats the correct activation due to it being an overcomplete basis and wanting sparse activations" i guess  

one problem is bands are still in the view of the composite activation. if we consider within a band, each "true feature" resides within their own seperate band which then they get added together. wait is that even true i guess not. band is actually the actual band that features will get added to the actual region in the hyperplane before they breach local non linearity.  

with sparse autoencoders. SAEs forces a relu, which allows less room (only one quadrant) to pack orthogonal features, makign them more basis aligned.  

there is a purely compression information theory point of view on SAEs. we don't care about the fact that SAE matches linear representation hypothesis (trying to decompose some true features the model actually uses) nor do we care it ignores non linearity bands. we just want some sparse code for activations that reconstruct it and we interpret those codes, because enumerating all direction magnitude pairing is impossible. trying to minimize codelength (the sparse feature activations) and codebook (the sae weights)  

# interpratability  

we need to ground interpretability to output eventually. changing it should change outputs. one possible approach could be: we find features near the end of the layer. and then we can go earlier where we don't have to directly predict outputs but can just predict the next layer of feautres. this mirrors the hierachical parallel check hypothesis in mindology. but currently we interpret everything in parallel directly according to outputs.  

# biological view

each activation is the rate of firing (each coordinate of the activation comes from one previous neuron) and each weight of an encoder neuron is the receiving synapse (number of neurotransmitters). in a 2d → 3d matmul, there are 2 previous neurons and 3 encoder neurons each with 2 weights. the 1st previous neuron fires to the 1st weight of each of the 3, the 2nd previous neuron to the 2nd weight of each.

# features and matmuls:  

a matmul, whether up or down projection, can always be pictured as dragging the basis. up projection drags each basis to a high dimension and down drags them to a lower dimension.  
how does matmul map features to the desired outputs? what are the limitations since matmul -> relu -> matmul can't be infinitely free?  
a feature, decompose it to a combination of coordinates. each coordinate has a set output? wait no im confused  

what if i think of a space not as a whole space but as individual directions and magnitudes and what they eventually become? local linearity is the bound?  
within locally linear regions we can decompose inputs into their coordinates, pass coordinates through the two transformations including relu, and get the direct out.  in fact. in some sense we can ALWAYS do this. relu only defines WHEN A NEURON GETS APPLIED. but wait neuron row view is not the coordinate view. also neuron row view is projection not decomposition!  

this introduces a split view i guess the neuron projection view and the space decomposition view. or rather the space decomposition view is basically the column basis view.  

well  

each encoder row neuron can be visualized as a hyperplane with a hinge. with say 2d x y input z output. then everything negative gets cut by relu causing the hinge which is also the decision boundary. and then decoder we can think of each output coordinate as a linear combination of that decoder row of the various hyperplanes. or we can think of all the hyperplanes z as coords and use each to scale a decoder basis. none of these give an intuitive space shift tho. maybe it's impossible? if it's "intuitive" maybe it can't be powerful?  

several views present here: space shift, hyperplane, encoder decoder, decision boundaries distances. fuck need to think through these  

PIECE WISE SPACE SHIFT! within a decision boundary carved polytope it IS a spae shift. we an pass x and y in and get a new x and y it is a drag. then we drag the boundary of the polytope to get where it's shifted to.  

side note. it is worthwhile to see what "intuition" even means. it means a correct and compressive understanding i guess that will later help with mech interp? you can get wide rules of what's possible what's not, like the geometric intuition of how without relu it's a linear line no matter what in the 1d to 1d case. want more intuitions like this. i want ALL possible intuitions like this regarding deep learning to be learnt by me. maybe there aren't even that many possible.   

a simple intuition check is: what is possible with this neural network and what isn't!

and some like 3 training data mlp 2d 3d relu 2d is possible to predict and some isnt WHY. 

btw we can normalize encoder weight and bias and times that number into the decoder and decision boundaries won't change. 

however piecewise linear space shift. doesn't capture the fact that nearby polytopes like tend to be alike in terms of shifting cause they're only one neuron's few influence apart. wait i guess they ARE always going to border each other still.  

without relu (2,n) encoder (n,2) decode is always jsut a (2,2) space shift. no matter how big n is. (btw what constraints does small n have?)  








# open questions

- what folds are possible in a mlp nd->md->nd if m > nd?. like i know how to conceuptulize a no relu spaceshift (dragging coords) but how do i conceptualize a thing with relu in between because it's not infinitely free.  

- why exactly can we rotate encoder and decoder freely if there's no relu between them? (in the superposition example this rotation is fine — in fact the only algorithm that worked. and in attention there's no relu either.)
- how would a row neuron view theoretically encode a polysemantic basis? the column basis view also wouldn't work cleanly with polysemanticity.
- maybe features don't exist — only "directionally coded meaning" exists. that might actually be the correct view, since it would be how things are decoded too.
- maybe there's some "co-definition" between encoder and decoder, and it's all just space scale.
