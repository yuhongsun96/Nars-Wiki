NARS is an inference engine that consists of the three major parts:
![Inference engine and reasoning cycle](https://user-images.githubusercontent.com/8284677/45256877-7d022d00-b36a-11e8-8ff3-b13db705cad6.png)
with these 3 components, inference can be understood as a cycle:
1. Select 2 premises from memory (Control mechanism)
2. apply the logic rules that apply (Logic)
3. obtain the results and put them back into memory. (Memory)

Since NARS works under the Assumption of Insufficient Knowledge and Resources, it has to maintain a max. memory capacity, which the Bag data structure, a kind of stochastic priority queue, allows for: 
![AIKR and Bag](https://user-images.githubusercontent.com/8284677/45257215-7eceef00-b370-11e8-8a8a-51990dafa8cf.png)
![Budget value](https://user-images.githubusercontent.com/8284677/45256919-4d075980-b36b-11e8-98c4-c028e87f1784.png)
The for us relevant operation will be a combination of select, then some usage we didn't specify yet, and then and Add call with the selected item to put the item Bag into the data structure, with Forgetting applied in-between.

Since NARS uses a term logic, memory is structured by the occurring terms, which can be compound terms and their components. Each such term gets a concept container associated with, which resides in memory, which is a Bag of concepts:
![Memory as a bag of concepts, concepts named by term](https://user-images.githubusercontent.com/8284677/45256897-db2f1000-b36a-11e8-8daf-79b530809ad4.png)

Besides being linked with other concepts by their component terms (using Termlink bag), such concept containers have a belief table, (similar to Bag but ranked by confidence) that stores sentences with judgment punctuation that have the same term as the concept is named by:
![Belief table of concepts](https://user-images.githubusercontent.com/8284677/45256980-4a593400-b36c-11e8-8243-75d8c65bc1c5.png)

The other data types:
![The data items](https://user-images.githubusercontent.com/8284677/45256951-049c6b80-b36c-11e8-8bfc-78baec482068.png)

Note: Input and derivation are always tasks. And when derived or input, tasks enter the tasklink bag of the concept with the same term, and all of its subterm component concepts.

The following example input:

![Example1](https://user-images.githubusercontent.com/8284677/45257254-2d732f80-b371-11e8-934d-f6f5a046dfcd.png)

leads to the following memory structure:

![Example1, memory view](https://user-images.githubusercontent.com/8284677/45257659-f3f1f280-b377-11e8-8b30-177115ef5e64.png)

And in detail, with the concept's internals:

![Resulting memory structure](https://user-images.githubusercontent.com/8284677/45257702-6b278680-b378-11e8-81ef-25bd329f6264.png)

The following deduction done by the selection of the concept animal, cat --> animal as tasklink (determining the selected Task), and animal --> being as termlink (determining the selected belief):
![Example1](https://user-images.githubusercontent.com/8284677/45256939-c901a180-b36b-11e8-985b-a5a3cc32ba84.png)

In detail, the inference happens through the following control loop:
![Control loop](https://user-images.githubusercontent.com/8284677/45257040-4aa5ff00-b36d-11e8-8e2f-efe6d66a5a9f.png)

Note here that 6 also happens for input tasks, which is how we obtained the memory structure resulting from the example.

Next tutorial: While the term and truth of the conclusion is decided by Non-Axiomatic Logic, how is the budget of the conclusion derived?
