# training dynamics?  

how many data is needed?  

we want to learn the features, and we want to learn the correct relationship of features with their related features and eventually the actual data  
for example, to learn the concept of a dog, we want to also learn fur, tail, and then we want pictures of dogs, different breeds, different colors, different angles, different distance.  

thinking in terms of two types of training: actual neural network training (data -> model) and mech interp training (activation -> features)  

algorithm is saying: this algorithm and size is enough to fit the neural computation to do this task. data and optimizer (training) is saying how do we get to the correct weight configuration.  
ever since you initalize the algorithm and size the "ideal weights" for the bench already theoretically exists. and in a mech interp perspective it already:q

really like the momentum as heavy ball anology. like its rolling down the loss curve. the momentum help it override a lot of the like tiny uphills.  

overparameterization helps (even if dead neurons exist) because it helps avoid local minima. even a single coordinate that gradually gets out of a local minima brinsg the whole thing along with it to a different place.  

# questions  

are different training data competing for limited features?  

the feature and feature to feature relationship. features are activations, relationship are weights?  

why adam? why momentum? uses so much vram is it really better? maybe test with a emnsit bench  


# some things kimi said 

                                                                                                                                                                                                                                    
   SGD noise is a feature, not a bug.                                                                                                                                                                                                  
   Small batches make noisy gradients. That noise acts like an explorer that kicks the optimizer out of sharp crevices and into flat basins. Large batches give cleaner gradients but tend to find sharper minima unless you add noise 
   or tune the learning rate. So "noisy" often generalizes better.                                                                                                                                                                     
                                                                                                                                                                                                                                       
   Flat minima generalize; sharp minima memorize.                                                                                                                                                                                      
   A flat minimum stays near the bottom of the loss bowl even if the data shifts slightly. A sharp minimum is a needle: tiny perturbations in weights or inputs make loss explode. Test data is always a perturbation of training data,
   so flat wins.                                                                                                                                                                                                                       
                                                                                                                                                                                                                                       
   Residual connections are iterative refinement.                                                                                                                                                                                      
   f(x) = x + small_change. The network learns a tiny correction to the identity, not a full transformation. That is why you can stack 100+ layers: the default behavior is "do nothing," and each layer only tweaks.                  
                                                                                                                                                                                                                                       
   Scale usually beats clever architecture.                                                                                                                                                                                            
   Given enough data, parameters, and compute, simple architectures often catch specialized ones. Architecture is inductive bias; scale is the engine. This is the bitter lesson.                                                      
                                                                                                                                                                                                                                       
   Warmup + decay control the search temperature.                                                                                                                                                                                      
   Early training: large steps / high noise to explore. Late training: small steps / low noise to settle. The schedule is a thermostat for the optimization process.                                                                   

