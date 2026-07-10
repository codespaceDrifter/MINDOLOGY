notes for the paper Avoiding Catastrophe: Active Dendrites Enable Multi-Task Learning in Dynamic Environments  
https://www.frontiersin.org/journals/neurorobotics/articles/10.3389/fnbot.2022.846219/full  

code implementations in:  
https://github.com/codespaceDrifter/svnapse/blob/main/algorithms/neuromorphic/pyramidal.py  

the active dendrite model has two components. the first is the active dendrite neuron. where each neuron has some dendrites that are a dot product with context vector and the abs max of them gets applied to a sigmoid which then scales the regular neuron. the second is sparsity with a k winner take all activation function. where each layer, the k highest value neuron activation gets preserved and everything else is zeroed. for a clearer implementation see above code implementation.  

the theory is that active dendrites help to deactivate or scale up the neuron depending on the context, which then determines whether it gets zeroed out in the following kWTA. this allows a single neural network to have different subnetworks depending on the context.  

we can get the context vector by either just one hot encoding a specific context or with a prototype vector. which is all that context's input vectors in the training data averaged, and in the test we just compare the new input to all the prototype vector and use the prototype vector closest to the new input.  

this resulted in greater performance in multi task RL learning and continual learning compared to normal ANNs and some other methods. the theory is that active dendrite models allows the network to differentiate different contexts so learing a new thing would not mess up old learnt things and different sub networks can both share abilities and differentiate. this is because only the winning dendritic segment and the winning neurons gets updates.  

this can be combined with another method SI: synaptic intelligence. a running measure of how important a synapse is for the loss of all previous tasks calculated as running total of gradient times weight change at each step. the more important it is the less it gets updated.



