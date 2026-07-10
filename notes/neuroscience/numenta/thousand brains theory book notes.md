this is a general note for the book. specific papers notes in this folder
papers at https://www.numenta.com/resources/research-publications/papers/


## prediction and learning
apical dendrites are dendrites that are further away from the soma. their activations cannot directly trigger the neuron to fire, but they can make it easier for a neuron to fire. this is used for prediction. the primed neuron will fire faster than other neurons and then inhib the other neurons. after this by the hebian rule the primed neuron and the neuron that primed it will have their connection strengthened.  

for example if say, (N for neuron)  
$N_{AB}$ is used to predict A->B. the input A arrives causes $N_A$ to fire which is wired to $N_{AB}$'s apical dendrites. (it also prediction primes several other neurons like $N_{AC}$ and $N_{AA}$  
then input B arrives, causes $N_B$ to fire which is wired to the proximal dentrites of $N_{AB}$ and causes a fast firing. and the connection of input $N_A$ and $N_{AB}$ is strengthened. also the $N_{AB}$'s firing inhibts some other neurons like $N_{CB}$ and $N_{DB}$ so they do not fire due to $N_B$ even tho $N_B$ is also wired to them.

## sensor, motor, reference frames

learning requires combining sensation and motion to map the complete 3d coordinate of an object in a reference frame  
grid cells, which are evolutionarily used to map locations, are also used to map places on a reference frame  
place cells, which are evolutionary used to recognize places, are also used to recognize specific feature at coordinates of reference frame.
in a cortical column, the sensor neurons and the motor send activations to upper decision neurons. this process of prediction and verification causes learning. the upper deicsion neurons also sends signals back to influence perception and learning  
complex concepts also have reference frames. for example 'bird' can have 'axis of movement' like beak length, wingspan, and color into different species. math equations can have axis of movement like different math operations into different expressions  

## cortical columns

the neocortex is made up of different structually identitical cortical columns. as opposed to the structually different old brain in charge of more primal survival functions, and in charge of more fundemental emotions and motivations  

each cortical columns has the sensor, motor, decision, back and forth loop to learn reference frames. each cortical column can learn partial or complete reference frames of an object. i.e. it could only have reference frames for one part of a coffee part (i.e. handle, body) or one sensory medium (i.e. vision, tactile etc). 

each cortical columns can know many objects but obviously not all the objects. every cortical column knows some objects (partially or completely). and they vote on which object it is via the top layer of long axons. when each object come in the cortical columns that think the recongized something sends signals to vote and they eventually reach a consensus   

for example say object A has 3 parts and there are 100 cortical columns (in reality human brain has 150,000 cortical columns). say 20 of them know object A. like maybe 6 of them know object A part 1 and etc. etc. when object A appears they vote via sparse population coded axon signals. the signals laterally reinforce each other. i.e. cortical column 1's object A signal stimulate neurons in other cortical columns that could lead to a object A prediction. they also inhibit other predictions. this way a majority consensus is reached. also this process reinforces the signal that recognizes of the object. which can then lead to further reasoning or influence sensor or motor neurons.

one implications is in machine implementation, this could be scaled parallely across many computers both during training (more cortical columns can learn more) and during inference (they only need to communicate in voting)
