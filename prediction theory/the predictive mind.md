notes for the book 'The Predictive Mind' by Jakob Hohwy  
headers are chapters names

## perception as causal inference
bayes rule: 
$$P(H \mid E) = P(E \mid H) * P(H) / P(E)$$ 
proof:  
$$P(H \mid E) = P(H \cap E) / P(E)$$  
$$P(E \mid H) = P(H \cap E) / P(H)$$  
$$P(H \cap E) = P(E \mid H) * P(H)$$  
$$P(H \mid E) = P(E \mid H) * P(H) / P(E)$$  

law of total probability: 
$$P(E) = \sum_{i} P(H_i) * P(E \mid H_i)$$  
proof:  
assumption: every part of E is covered by some non overlapping hypothesis $H_i$  
$$\sum_{i} P(H_i \cap E) = P(E)$$
$$\sum_{i} P(E \mid H_i) * P(H_i) = P(E)$$


P(E) is calculated as some number that normalizes the sum of the $P(H_i \mid E)$ to 1. in reality it's often abbreviated  
when considering perception and object recognition we mostly just write $P(H \mid E) = P(E \mid H) * P(H)$

perception follows bayes rules, object recognition given some evidence is the probability that the evidence causing probability times the self probability  

for example, if i hear a loud sharp sound. what is the sound? it could be either  
a mechanical alarm clock: high evidence causing probability due to sound accuracy. low self probability cause i dont own an alarm clock  
a bird chirping: low evidence causing probability because birds are not that loud. high self probability cause birds chirp outside.  
phone alarm: high evidence causing probability cause sharp sound match. high self probability cause my phone alarm rings every morning.  
so i recognize that sound as a phone alarm ringing.  

i do not know i am doing this. the process is unconscious and fast, at a neuron level. 

there are different levels of regularities. small scale regularities like object persistance have short time scales and lots of detail. large scale regularities like economic patters have long time scales and low detail. the prediction happens via a range of hierachies talking to each other. top down regularities priors predictions guide low level perception and low level perception corrects top down predictions

## prediction error minimization

the real physical world is based on some equations at a low level which form some patterns at high levels. (or maybe the world is just the world and this is just a correct model of the world). the actual data we get is combined with different contexts which add noise. 

true generator function + random noise = actual data  

the mind should try to extract this true generator function or some patterns from the data to be used later to think (and predict and perceive).  

in bayesian terms. P(e|h) the mind contains the hypothesis which is higher level concepts which predicts the evidence which is the outside world received through sensation.   

learning is supervised by the world itself rather than a external labeler or previous priors self regression. this avoids the explanatory circle where inference shapes priors and prios shape inference.  

mutual information: if neural layer n is to represent some cause c in the world ideally n fires when c occurs. n and c would be strong predictors of each other and their mutal information will be high. the brain only has access to sensory input u as a proxy of c so it tryes to max mutal information between n and u.  

prediction error = surprise + perceptual divergence.  
surprise is how unlikely the event is given your past experience (the actual distribution of the past events you've been through). this is based on your model of the world.  
surprise = - log (e|m)  
perceptual divergence is the difference between p(h|e) and the true posterior based on surprise   
perceptual divergence = Kull-Leibler divergence  

surprisal is the real theoretical lower bound for the amount of error. like if we were to count events perfectly how rare different states are. the selected hypothesis is not perfectly equal to this so when trying to minimize prediction error, we try to minimize perceptual divergence so prediction error gets close to surprise. an actual surprising event should be surprising and cause high prediction error but actual common events shouldn't.

we use prediction error to learn.

example diagram:  

layer (higher concepts) --> prediction --> layer (object recognition) --> prediction --> layer (sensation)  
layer (higher concepts) error <--  layer (object recognition) error <-- layer (sensation)  

top level is abstract. bottom level is input.  
the error is the prediction minus actual activation. when we try to minimize that difference through learning we are maximizing mutual information. also each layer represents a hypothesis and the prediction is like P(h|e) strong. so when we learn we maximize the correct hypothesis' P(h|e). 

what you actually experience is a combination of the top down constructed prediction and bottom up error signal.  

