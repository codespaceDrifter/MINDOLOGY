TO DO:

hebian learning mechanism
action sequence pattern matching past actions for quick gen
compare hebian learning with gradient descent
hippocampus learning?
define concrete dataset and tasks and algorithm to study different theories
representations form components. like i would say break a person's mind into components. major themes of them. like how utopia and laziness and whatever is a theme of me and some personality trait or hobby might be a theme (component) of another person. doesnt feel like just representations.

MAJOR NEUROSCIENCE MECHANISMS NOT YET UNDERSTOOD:
- neuromodulators (dopamine, serotonin, norepinephrine) - they CHANGE learning rates and rules globally, not just signals. meta-learning.
- basal ganglia + dopamine - action selection, reinforcement learning, reward prediction error. "should i do this or that?"
- hippocampus - episodic memory, one-shot learning, very different from cortex. "what happened when where"
- cerebellum - motor prediction, timing, "my arm will be HERE in 100ms"
- oscillations (gamma, theta, alpha) - timing coordination across areas, binding, attention. rhythm matters not just rates.
- sleep + memory consolidation - replay, transfer from hippocampus to cortex
- attention - gain modulation, "turn up volume on this, ignore that"
- working memory - hold info online for seconds, sustained firing, prefrontal cortex

ML mechanism not yet understood:  
meta learning  
how does word2Vec relate to abstracted relational representation maps.  



# a rough draft for MINDOLOGY.md. 

# the mind

to gather all my thoughts and identify what needs to be learnt. writen poetry core.  

the mind is a neural computer that could be conscious.  
a neural computer uses dot products and relus to form representations and transformations to model the world.  
consciousness is the subjective experience that biological neural computers like human brains have but current silicon neural computers like gpus probability don't have yet.  
meaning wise, some minds are conscious, and consciousness is where all meaning in the world comes from, but that is more about utopia.md and we don't know why minds are conscious and won't discuss consciousness much here. and we assume artificial minds are not conscious yet.  

i write this both for deep personal interest in understanding the mind and to build primy, the aligned and capable agi system to bootstrap utopia. this is written from general to specific. covering broad concepts, theories, biology, psychology, frontier AI, mech interp, then my own research.  


# functionality of the mind 

functionality wise, the mind tries to generate the best action sequence for a goal given the enviromental information.  

i will define some concepts.  

aesthetics: the end goal of a mind. it's "motivations and wants". for most organisms it's mainly the evolutionary mandate. to survive and reproduce. but it's also consciousness states. like pleasure. or whatever aesthetics. like promoting freedom or beauty or curiosity. a model can have multiple aesthetics.  

world state: just a state of the world. a situation of the configuration of matter that exists. different world states can be scored and ranked by aessthetics and given an aesthetics score. this is called reward in RL terms.  

enviromental information: the model is connected to sensors that send it information. could be eyes or cameras for vision for example. in a modern context sensors can be like a camera or lab microscope recording halfway across the globe im watching via internet. it is limited from whatever sensors the model has, either eyes or remote cameras or whatever.  

actuator: the model is connected to actuators it controls. like a brain to an arm. in a modern context actuators can mean control over distant things like drones or money or people. an actuator is a part of the world state wich a "command" score of how much control the mind has over it. like the body has a high command score of close to 1 but the for example "the masses influenced by the media the agent produces" has a lower command score.  

action sequence:  after an action, the world is changed into a new world state. several action choices form a action sequence, and different action sequences end in different world states. these world states can be ranked by aesthetics. and the mind's job is to generate the action sequence that maximizes resulting aesthetics score. note that this is more like dict modification than branching tree, since a lot of action sequences will converge into the same world state. like whether i lift first or do physics first today doesn't matter at the end of the day if i do both it's mostly the same change in world state. if you imagine a muscle mass field with a float score and a sort of physics knowledge field with some representations learnt.   

representations: the mind has to model the world to simulate (aka predict) the world states actions cause. the mind has practical compute (speed) and capacity (ram) limits so it cannot achieve a full simulation of every subatomic particle and every slightly different action sequence towards the end of the universe. therefore the mind needs abstract the world into representations that still can simulate the world with as little error as possible. representations are the neurons firing speed (activations in ANNs). for example, 'leopard' is a representation over many different animals with shared characteristics such as running fast and being a primal predator of humans. for example '7' is a representation of a abstract number no matter what it is counting or how it is written.     

transformations: the mind transforms these representations to move the inner world model forward for prediction and to traverse abstraction scales. transformations are done by the neuron synapse (weights). for example, when reading a written '7', first the 'slanted line' representations and the 'horizontal line' representations fire and then in a later neuron layer that receives them as inputs and fires '7'. for example, seeing a 'leopard' transforms into a later world state of 'being eaten' which probably means the human should grab fire or sticks.  

knowledge: a abstracted relational model of the world formed by learnt representations and transformations. will explain more in later section.  

thinking: using current learnt model to simulate the world state different action sequences end in. includes thinking speed (in a given amount of time how far can a mind think) and thinking capacity (how many representations can a mind use at once). will explain in later section.  

capacity: how much representations can be stored (memory). thinking capacity is how many representations the working memory can hold at one time (activations size). knowledge capacity is how many represenattions the knowledge can learn in total (weight size).  

speed: how fast transformations can happen (processing). thinking speed is how many transformations in a given time the mind can do (ghZ / neuron firing rates). knowledge speed aka learning is how fast the mind can store a new representation into weights (training time)  

mind: a neural computer that could be conscious, that uses represenattions and transformations to calculate the optimal action sequence for an actuator to achieve the world state with the highest aesthetics score.  

agent: just the mind and the actuator as a whole.  

god: think of a theoretical being, the most powerful being possible under the laws of physics. what are the gaps between a squirrel and god. 0: actuator gap. the actions god can take (with its robot legion) such as building fusion reactors or manipulate atoms to make nanobots, a squirrel cannot do.  1: information gap: god knows the position of every particle and the squirrel is limited by the immediate surronding. 2: knowledge gap. god has the knowledge of every useful theory from physics to biology to computation to mindology in its mind. where as the squirrel or the average human doesn't. 3: thinking gap. god can think at infinite speed, simulating many possible action sequences with perfect accuracy and to a very far time ahead. (theoretically with perfect information and infinite thinking god doesn't need knowledge cause it could just simulate the world with quantum mechanics perfectly for the timeline of every minute action difference).  

god action sequence: say we remove the godly actuator and godly enviromental perception and just put the mind of god inside a body of a squirrel. (theoretically mathmatically controlled cause obviously the brain of a squirrel wont fit a god mind no matter how much learning it does). if a all knowing, infinite processing power god took control of the squirrel. what would it do. well honestly for a primitive squirrel not much. but for a modern squirrel it could legit go to a laptop. type in some code to hack into some money and compute. type in the perfect agi code that is high capability and high alignment to its aesthetics. say world utopian dominion. and start bootstrapping the timeline of the entire universe. but back to the primary situation, without computers or the possibilty for modern civilization before the squirrel body dies. the god action sequence would know where exactly to go to find the nuts and water and where to bury it to and where to find mates and how to rizz them up as best as a squirrel can rizz its way up the mating hierachy.     

opmital action sequence: what the model generates as the best action given limited thinking power and limited knowledge. so the human with a computer but don't know how to code the aligned agi up. maybe it tries to learn about how the mind works and generates a direct and honest plan, this is the optimal action sequence. so it's like given the aesthetics and knowledge and thinking and actuator and information what is the best action sequence this mind could have generated if it had perfect motivations and consciousness states and work ethics and i guess the devout pursuit of the divine assuming the aesthetics is beautiful, what is the action sequence the mind would have generated.

actual action sequence: the sequence that is really done by the mind. note that the optimal action sequence and actual action sequence are different. the mind has many problems, laziness, depression, various motivations competing, learnt harmful tendencies, etc. note that also most actions have short timelines. and they are just based on quick past pattern matching stored in weightsrather than a complete intentional tree search of action sequences.   

optimality: how similar is the actual action sequence and the optimal action sequence. the mind has multiple aesthetics that are competing for the actually chosen action sequence. this could be problematic for the mind, like when it's trying to work it gets reward hacked by youtube instead. the optimality of the model determines it's ability to actually execute the optimal action sequence it thought of.  

evolution and learning: evolution determines the hardware of what speed and capcaity cap a model could have for thinking and learning. as well as some hardwired aesthetics. more specifically, how neural computation is mapped, i will discuss in the next section. in ANN this is algorithm design. learning is constructing the actual representations from the world. note that learning can also shape aesthetics and in some sense increase speed and capcaity maybe a little bit. in a ANN loss landscape sense, the algorithm (evolution / design) determines the shape of the loss landscape and the learning (training) determines at what point of the loss landscape does the weights of the model lie. a model that is too small or poorly designed won't converge to a low loss no matter how much learning done.  

# neural computation

why is neural computation needed in the first place? there is mainly two types of computation: symbolic and neural. symbolic computation, like math and physics, uses symbols and rules. which are clearly numerical and defined. for example symbol like mass or charge or size, and rules like newton's laws. when coded up in a computer simulation, can simulate the world. physics can also have different abstraction levels, as we see newton's laws breakdown at the atmoic or astronomical scale there are other symbols and rules. symbolic computation is like classical programming without neural networks. so what can symbolic computations not do that neural computations can? let's think about some examples. example: recongizing a dog. example: walk on uneven domain. example: tool making like making a bow and arrow. example: inventing new symbols and rules (physics/coding). so basically neural computation can encode the real complex world into abstracted representations and transformations.   

so what is the computation best for neural computation? we know the answer from evolution and ANNs it seems to be dot products and relus roughly.dot products and relus is the simplest possible operation that takes a complex noisy input, similarity checks it with a learnt representation, and fire if it is similar enough.  

dot products and relus are stacked in layers. within a layer it is a parallel check of many concepts constrained by capcaity. different representations can be found in parallel with a lot of dot products over the same inputs and the irrelevant ones relued. and representations that are more abstract that build on other representations can dot product previous representation activations. different layers can be chained together recurrently sequentially to build representations of representations at higher abstraction and to predict further, constrained by speed. for example say you want to identify the digit '7'. first you run a horizontal line checker and a slanted line checker dot product over all pixels, then you run like a horizontal line above slanted line checker over the previous layer. obviously in a real model this would be more nuanced but this is the rough idea. for another example, say you're hungry and there's a river between you and a fruit tree. layer 1: 'river' + 'deep' → 'can't walk across'. layer 2: 'fallen log nearby' + 'log floats' → 'bridge'. layer 3: 'cross log' + 'reach tree' → 'food'. each layer simulates one step further, chaining representations into an action sequence that hasn't happened yet.  

check scores are not binary but floats. 

an ideal representation is a dot product that activates on as much of it's previous layer inputs as possible while still being useful for prediction. for example a curve representation, it doesn't care where the curve is in the picture it just activates when there's a curve. and a circle representation, it's inputs are the curves and it just activates on some currently oriented curves. same with representations like dogs or balloons. as regards to why these representations are chosen, it's because they can activate the most. for example, say why "balloon" and not "balloon with a little bit of sky and leaves around it". it is because not only does the latter disrupt the sky representation and tree representation, it fires less often than a pure balloon representation, which can also fire for balloons indoors.  
the constraint of weight capacity and thinking capacity and speed actually helps the model learn these good representations that can generate better to different new scenarios. in ML this is called preventing overfitting.  
(here we assume sparse neural coding where each neuron represents a concept. this is probably not correct and neurons are probably population coded. see anthropic papers on superposition)  

the optimal action sequence needs to be mappable to the actuator and the optimal thought sequence needs to be mappable to the mind. in biological terms the body is a constarint and the brain is a constraint. but what is the optimal thought sequence that is the question.  

representations and transformations co define each other. they must exist in pair or groups. because an activation is entirely defined by the weights of neurons receiving it.

optimal thought sequence: abstract and accurate modeling of the real world that takes as few representation and transformations as possible. correct purging of bad action sequences with a good representation that can sort of encode future aesthetics scores?

# high level thinking  
what about long horizon high level thinking that isn't just a forward to back? maybe they are mostly upper level concepts. what causes this in the brain. i think there are two causes (kind of a murky divide)  
1: causality. by causality i mean timing. the brain naturally predicts and learns the thing that happens next with stdp. and that learnt transformation causes high level continuous thought. cause predictive coding / stdp just reinforces whatever chain of representation firing causes the final output. so like (wait is this true?) . so like if it's high -> high -> high -> mid -> low that matches some future (or i guess a mid or high level could be a future, think math, i can know the correct answer i dont have to see it), then that chain of transformations is going to get reinforced. obviously this is directly useful for the purpose of the mind: to simulate action sequences and how they affect world states.  

2: association. this could be caused by similiarity of representations in their activations. like paint on a web canvas they just spread to nearby stuff ok bad anology but. similar vectors can just cause each other to fire perhaps cause it's only a few neurons apart. or maybe this is co-occurence. so different vectors that happen together a lot, like fire truck and red, they independently parallely fire together a lot which stdp picks up.   

also the association one might be useful to explore similar ideas when problem solving. similarity in design space is a kind of like time forward tree search. and it could be naturally done with just slight changes to the activation, if activations are grouped by similar meanings numerically.  
so it seems "ideas just come to you" maybe it's literal neural noise or the subconscious influence from other ideas.  



# different theories of neural computation  

now we understand more specifically how neural computation works.  

i will here cover all major existing theories of neural computation. they might conflict or compliment each other but here i just explain them. i will unify my own theory based on them in the later section.  

## population coding  

## linear representation hypothesis  

## representation combination

leopard + run = fast. how. how did i query that.  

# unified theory of neural computation



# learning

learning vs evolution. algorithm, loss function, and some parts of weights is evolved. most of weights is learnt. in ANNs basically what was evolved originally have to be designed like the algorithm and loss.  


# the biological brain  

the biological human brain is the only existance proof of AGI so we study it further here. including more specific neuron dynamics, brain structures, neuromodulators, etc.  



# network math

what is network math. what do i know. i know about the hierachical abstracted features how each one should help predict the ones below. i know about thinking as the perhaps features mapping to each other causaully to model the world. what is network math. some mathmatically conceptualization of. activations and dot products. WHY. why activations and dot products. well. on the first thing. dot products are similarity finders that can extract features. that is the abstraction level. on the seocnd thing. perhaps. since dot products are weights that are learnt they can be learnt like causality temporary relationship between features. they are both. in some snse. memory in the classical sense. what leads to what. but more fuzzy. more fault tolerant. what about the other things. linear representation hypothesis. well ok beyond fuzzy it is also about strength combination of different features. 
things relate to each other vape makes me want to throw up but so does confusing ideas i have not fully written down. and they add together. is that it. 
why relu. to make irrelevant feature finders not affect the activation. like if some feature is not present i don't want there to be a negative value in the activation that will affect later computations.  
also all of this is population coded rather than single neurons. each feauture is an activation rather than a coordinate.  

composition. different coordinates can encode different parts of meaning. for example if small, furry, and fast means mouse, maybe it is 3 parts of an activations that occupy different coordinates, and if i switch the small part for big it would be detected later as cougar instead. if each of the 3 parts is the full activation added together, it would be harder to detect.

what about algorithms or rather circuits what are they like ICL and maybe mesa optimization
