the specific neuron physics stuff or population stuff will be in seperate folders this is just some general facts

# human brain facts

neuron count: 8.6 * 10^10  
average synapse per neuron: 5000  
average firing per second: 1  

approximations
weights count: 10^15  
ops count: 10^15  

by fp 16 apprximations:
weights: 2 petabytes
flops: 2 petaflops

in 5090s:
weights: each has 32 gb vram. need 62500! gpus. not pheasible. 
flops: each has about 500 tflops. about 4 gpus. 

so maybe i do a single computer with 8 5090s and 2 petabytes of ram.

