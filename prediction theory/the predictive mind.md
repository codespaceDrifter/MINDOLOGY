notes for the book 'The Predictive Mind' by Jakob Hohwy  
headers are chapters names

### perception as causal inference
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

### prediction error minimization

true generator function + random noise = actual data  



