# attention  

we try to understand attention here as a general module of the transformer rather than specifics of various attention algorithms  

# why transformers can read in parallel   

a very interesting question is: why is it that a transformer can read any length of large text (book, codebase, papers, etc.), given enough vram, in O(model depth) time, when a human can only read it in O(text length) time?  
a human reads one word at a time. but a transformer reads every token at once in one forward pass. allowing crazy capabilities such as reading an entire book in minutes or a codebase in minutes. when it would have taken the human hours.  
why is this possible? is this some sequential vs parallel problem?  
humans do have parallel processing. such as viewing a scene, one can very quickly know all the objects there. but things that have a sequential order, such as text, or a movie, human needs to process it sequentially. but if you break a movie into individual frames and audio, a multimodal transformer can also view it in a single minute.  

not going into algorithmic specifics, at each token's attention layer, after causal masking, it attends to all previous tokens. and at each mlp layer it reasons a bit on what information it has currently. each layer adding to the residual stream.   
why this is possible is to picture a single transformer model as a TEAM of identical minds working together rather than a SINGLE mind.  

this is the human analogy. say you have only an hour to read a very long book or a code base. you have a team. each person is assigned a paragraph to read sequentially. so say person 0 is going to read paragraph 0, person 1 is going to read paragraph 1 (and only 1), etc until every paragraph is assigned to a different person. and the people cannot talk to each other.
we start with a thinking session where people just read the paragraph they have.  
then we do a note sharing session. where each person writes an abstract of what they want to share (K), a note containing what they want to share (V), and decides what information they need (Q). then each person get all notes of those who read paragraph previous to them (person 1 gets notes from 0,1. person 2 gets notes from 0,1,2, etc.). they can decide based on what they want (Q) and the abstracts (K) to determine how much they want to consider the information shared (V). then they have another  
in the next thinking session, they can utilize both the notes they gotten and the paragraph they already read. and they can understand the entire text better and for the next note sharing session, they can think of a better information they need (Q), what they can provide (K), and what they want to share (V).  

note that for a more accurate analogy each human is an identical clone of each other and is only assigned a single token rather than a entire paragraph.  

we use multihead attention. so at each token it can "look for", "offer", and "note down" different things in the bulletin board, and all the head's information will be added together.  

# multiactivation

a transformer is able to do this because it can, with enough vram, can process as many batched token inputs as possible. essentially copying its own model into like a team of identical clones. and with enough vram it can store as many past notes (KV cache) as possible. note another thing it can do is external computation based on activations that are parallel but don't involve weights, like the Q dot K interaction, like each person can read all notes in parallel and decide how much importance to assign to them. note that rather than "note integration" a transformer just adds all V together with respective scaling.  
rather than "weight copying" or "cloning into a team" i will just say "parallel apply" from now on cause that's what actually happens, but it was a good intuition.  

humans can also parallel apply to some extent. humans read one word at a time, not one letter or one curve and edge at a time. but the total ability to parallel apply is constrained by perhaps the total neurons and their activation in the brain.  
each neuron, biologically and in silicon, are made up of weights and activation. think of activation as memory, or a workspace. perhaps a simple explanation is more complex ideas require more activations. and a human brain with its limited neurons can only have so many activations and so many ideas, thus making reading a paragraph at once impossible, needing constant sequential compression.  
but a transformer, each neuron can have multiple activations, as many as vram allows in fact. this "one neuron multiple activation" framing is better than "copying its neurons many times"  

# QK circuit OV circuit  

we can multiply QK and OV together to see what features it wants to attend together and what information those output  

by the laws of associativity of matmul:  
> $$A = x_0 W_Q W_K ^T x_1^T = x_0 (W_Q W_K ^T) x_1^T$$
> $$o = x_0 W_V W_O = x_0 (W_V W_O)$$

note both these are per head. $W_Q, W_K, W_V$ is (embed_dim, head_dim), and $W_O$ is (head_dim, embed_dim)  

using some feature decomposition, (SAE or CLT or WCC), we can use these two formulas to see how attention interacts with features.  
for QK circuit, we can directly calculate the rough attention score (pre softmax pre scaling) of any two features  
for OV circuit, we can directly calculate what output feature combination the input feature becomes  
so for each head we can get a table of feature attention scores and a table of what each feature becomes. note the full output of o would be scaled by A. so we structure the table in the perspective of the attended to token passed into K. we list the top i.e. 5 features passed into Q and their respective attention scores, and the top i.e. 5 features the OV circuit transforms it to and their respective projected magnitudes  

attention is linear other than softmax, unlike MLP. because attention is communication and mlp is thinking. and in communication you can have your message transformed but relu just makes too many of it 0. you want QK to be able to get negative attention scores, and you want VO to be able to reach negatives so they can have negative interactions when added together.  
