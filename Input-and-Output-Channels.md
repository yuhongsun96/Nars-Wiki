Input and Output to the system happens in the form of Narsese statements. Currently OpenNARS support two types of I/O Text and Vision

## Text I/O
Narsese statements as ASCII text can be input/output between user and system or between multiple systems:

### User interaction
When OpenNARS runs in terminal only, [described here](https://github.com/opennars/opennars/wiki/Quick-Start-Guide), users can input statements in the same instance of the terminal, the derived Narsese statements will output to the same terminal.
Using GUI, it is easier to interact with the system. There is an input window in GUI for ASCII Narsese statements to be input by user, similarly an output window shows derivation statements of the system. Look [here](https://github.com/opennars/opennars/wiki/Graphical-User-Interface) for GUI description.

While system is running, user can save derivations and system snapshot to the file, called [experience file](https://github.com/opennars/opennars/wiki/Experience-file:-Format-and-Usage), such that this file can be an input to OpenNARS at a later time and system will resume the derivations.

### Inter System interaction

Ability to exchange derived tasks between different NARS instances working on problems whose evidential spaces intersect allows considerably accelerate the learning process. Starting with OpenNARS 3.0 communication through UDP protocol is supported using Java built-in serialization techniques. Each Narsese sentence is being serialized as a char byte sequence, packed into single UDP packet and sent over the network. NarNode.java class is the only class responsible for network communication between running NARS instances. It serves as a middle layer between the front end application and Nar.java class that actually responsible for programmatic input/output of tasks.

### Programmatic Interaction with OpenNARS (Description of Nar.java class)

There is also a possibility to communicate with the system bypassing NarNode class and UDP communication. For that purpose Nar.java class has following methods:

1. addInput() – allows to add single line input to NARS. Once input is receives, it is being
parsed and inserted to the system
2. addMultiLineInput() – allows for entering multiline input.
3. addPlugin() – allows to add input from plugin such as SensoryChannel, Operator or Emotions
4. addInputFile() – add input from supplied file with list of statements 
5. start() – starts derivation process
6. stop() – stops derivation process
7. cycle() – execute one inference step
8. cycles() – execute specified number of inference steps

These methods are helpful in communication with the system through custom designed API or any
other options.


## Vision I/O
OpenNARS comes with a prototype implementation of a vision channel using the sensory terms (and perceptive terms partly implemented) as presented in our publication ["Perception from an AGI Perspective"](https://cis.temple.edu/~pwang/Publication/perception.pdf).
The vision channel takes in events talking about the brightness level of pixels at given coordinates:

<{M[x,y]}  --> [bright]>. :|: %b;0.9%

where x and y are the x and y-coordinate each between -1 to 1, and b encodes a brightness level between 0 and 1 as a truth frequency value.
Once an event for each input pixel has been received, or a pre-defined duration of time has passed, the brightness information that was gained by the vision channel will be summarized into a 2D truth value matrix {M_New}, a so called sensory term. This sensory term represents a prototype that will be matched to the existing stored prototypes {M_i} by pixel-by-pixel based comparison using the comparison truth function, finally summarizing all the obtained truth values with revision, obtaining a single truth value TruthMatch (for the highest TruthMatch truth expectation spatial arrangement offset (ox,oy), for comparison of {M_New} with {M_i}, to achieve translation invariance). The closest prototype will then be entered as

<{M_BestPrototype} --> [bright]>. :|: TruthMatch (ox,oy)

into OpenNARS (or the next higher-level sensory channel in the hierachy if such one exists) and the budget (so far just an amount of occurrences counter) of {M_BestPrototype} will be increased.
Also {M_New} will be entered as a prototype with initial budget (so far the initial occurrence counter being 1), kicking out the prototype with the lowest budget (so far simply the prototype with the lowest amount of occurrences) if at full capacity, giving {M_new} the chance to become an useful prototype on its own sake. 

So in total the role of the vision channel is:
1. Accumulation for instance, if the channel is of dimension 50x50, 2500 pixel-level events into a 2D truth value matrix (sensory term).
2. Searching for the best to that sensory term stored prototype, increasing its budget and taking spatial offset for the best matching into account.
3. Creates nevertheless a new prototype for the new observation (kicking out the worst existing), to allow it to become a useful prototype on its own sake.
4. Reporting of the best matched protoype to NARS, like <{M21} --> [bright]>. :|: (ox,oy) where M21 is a matrix term and the spatial match-location (ox,oy) is encoded in meta-data.

Point 2,3,4 is shared with Adaptive Neuro-Symbolic Network agent ANSNA for [matching events to concepts](https://github.com/patham9/ANSNA), and extending the principle with the adaptation of prototypes similar to ["Conceptual Interpolation"](https://github.com/patham9/ANSNA/wiki/Concept:-Conceptual-Interpolation) will be a future topic for improving the vision channel, also using SDR's instead of 2D truth-valued sensory terms is a theoretical possibility.

Regarding (ox,oy): This meta-information potentially allows other perception operators to build spatial relationships such as 

<(*,{M_30},{M_10}) --> leftOf>. :|:

meaning the vision channel allows for a equivariant representation rather than just an invariant one, not loosing the information of the absolute position of the match within the sensory channel. However such a mechanism is not present and maybe unnecessary, as the movement itself can encode that information, as described in our paper, demonstrated by the following sentence:

<(&/,<{M_30} --> [bright]>,^right({SELF})) =/> <{M_10} --> [bright]>>.

Also syllogistic inference on vision channel results are possible, as described in the paper, using abduction and comparison by pixel-by-pixel based comparison between two premises, that is

<{M1} --> [bright]>.

<{M2} --> [bright]>.

obtaining

<{M1} <-> {M2}>. revision(pixelwise_comparison(PixelTruths(M1),PixelTruths(M2))

and also abduction:

<{M1} --> [bright]>.

<{M2} --> [bright]>.

obtaining

<{M1} --> {M2}>. revision(pixelwise_abduction(PixelTruths(M1),PixelTruths(M2))

but note that this is happening by general inference control and not in the vision channel.

Additionally in OpenNARS-Lab there exists a Webcam example in the Launcher that demonstrates initial use of perceptive terms (and the learning of such procedural sentences), that is, sensory terms with a focal point that can be moved by operator invocation as described in the publication. So far the system automatically places its focal point at the areas within the image that change the most, re-identifying patterns it has seen before as well as learning which changing patterns likely cause others to appear soon in respect to the movements that occur, demonstrating the kind of procedure learning that is so central to explaining human eye movement and active perception in general.

Issues: Compositionality, hierarchical composition should be better captured by allowing prototype trees, ultimatively allowing for a feature hierachy, as a zoom operator alone might not suffice, also pixel-by-pixel comparison is quite limited.

