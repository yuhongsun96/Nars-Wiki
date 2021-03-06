
### Introduction

Every intelligent system needs a mechanism to achieve various goals. In NARS, this function is mainly carried out as procedural inference, which is inference on beliefs, events, goals, and actions.

In NARS, the system's beliefs are represented as Judgments, each of which is a Narsese sentence, containing a Statement as content, and a TruthValue indicating the evidential support to the content. An event is a special type of statement whose truth-value is defined only during a certain period of time.

A Goal is similar to a Judgment, except that its content is an event and the "TruthValue" in it is not really the truth-value of the content, but its "degree of desire", or "[desire-value](https://github.com/opennars/opennars/wiki/Desire-Value:-Definition-and-Examples)", which is defined as the truth-value for the event to imply a virtual "desired situation".

An Operation is what the system can do to cause changes in the outside (or inside) environment. Logically, it is an event that can be "realized", i.e., can be made to happen, by the system.

As a general-purpose system, NARS can have any belief or goal, as far as its content can be expressed in Narsese, and they change from time to time as the system runs and interacts with its environment. On the contrary, in any concrete implementation of NARS, all of its actions are composed recursively of operations, with a constant set of built-in Operators.

The inference rules work on goals and operations in the same ways as they work on events in beliefs.

***

### Operations

Each operation consists of an operator and an argument list, like Op(A1, ..., An). From a logical point of view, it is a statement <(*, {SELF}, A1, ..., An) {-- Op>, where the subject is a product indicating a temporary relation between the system itself and the arguments, the predicate is the type of relation, and the copula is Instance, since operations are countable.

In the current implementation, adding an operator into the system means

* to add one line in nars.operation.Operator to register it to the system,

* to define a class in nars.operation that extends Operator, with an "execute" method to invoke external "sensorimotor" mechanism that carries out the operation on the given arguments.

In the current code, there are a few dummy classes in nars.operation for testing purpose.
In this way, in the future NARS can serve as the mind of a robot, or a software agent that uses various software and hardware as tools. Ideally, only the nars.operation package needs to be changed when NARS is equipped with a different sensorimotor mechanism. Please note that in NARS, sensation/perception will be carried out by certain actions, so the nars.operation package handles both input and output through sensorimotor.

To the system, the meaning of (the content of) beliefs, goals, and actions are revealed by their relations with other terms, except that for actions it also include their relations with sensorimotor procedures, which can not be fully expressed in Narsese. Especially, the meaning each truth-bearing term (i.e., statement) is mainly revealed by its sufficient conditions and necessary conditions, some of them have a temporal attribute. For each action, its preconditions and consequences are represented mainly by implication and equivalence statements about the action.

To make compound actions from component actions, the two major term operators are "sequential conjunction" ("&/") and "parallel conjunction" ("&/") (see [Temporal Inference](https://github.com/opennars/opennars/wiki/Temporal-Inference)). When applied to operations, they mean "sequential execution" and "parallel execution", respectively. Combined with sufficient conditions and necessary conditions, they can form all kinds of "programs" from the "instructions" provided by the operators.

***

### Goals

At any moment, the system normally has multiple goals to be achieved. They are processed in parallel in a time-sharing manner. There are two types of goal: original goals are directly imposed on the system by its environment; derived goals are produced by the system itself from goals and beliefs via [backward inference](https://github.com/opennars/opennars/wiki/Backward-Inference-in-OpenNARS). The two types of goals are treated by the system as the same for almost all purposes.

When a new (original or derived) goal arrives, the system does not immediately start to find ways to achieve it. Instead, it is pre-processed in the corresponding concept, where the following factors are considered:

1. The [desire-value](https://github.com/opennars/opennars/wiki/Desire-Value:-Definition-and-Examples) of the goal is adjusted according to its relation with other goals. When the same goal gets several different desire-values from different sources, the revision rule is used to get an overall desire-value, which indicates whether the system really desires it, when all available evidence is taken into consideration.

2. The truth-value of the content of the goal is checked. If the goal is already achieved, the system does not need to do anything. The difference between the expectation value of the content and the expectation value of the desire is used as the "degree of satisfaction" of the goal.

### Decision making

This pre-processing of a goal is a decision-making process, by which the system reaches the decision on whether to actively look for a way to achieve the goal. As a special case, if the goal is an operation, the process will decide whether to actually execute it. If a goal passed this stage, it will get a [budget value](https://github.com/opennars/opennars/wiki/Budget-Value), and be worked upon in the following time accordingly.

In decision making, the plausibility of a goal is not directly evaluated, but implicitly. If the system cannot find a way to realize a goal, its priority will be gradually decreased by the attention mechanism.

In this way, the system doesn't consider each goal by itself but attempts to reach an overall optimal solution for all of its tasks, by compromising among their requirements.

Beside deriving goals from existing goals, NARS can also turn sentences of other types into goals. For example, question **"G =/> ?x"**, whose answers reveal the consequences of G, contribute to the desire-value of G as a goal. This is like to get knowledge by experiments.

***

### Examples

The general guideline for how to use the examples is described in [Single Step Testing Cases](https://github.com/opennars/opennars/wiki/Single-Step-Testing-Cases).

This group of examples shows how NARS works step-by-step to achieve goals by executing operations. Since the current focus is the logic, all control-related issues will be omitted. Most of the input tasks use default truth value, except those where the results are sensitive to the accurate truth values.

#### Example 1

The first example shows how the system (as a robot) uses procedural inference to achieve the goal of opening a door.

It starts with 9 input sentences, and they are roughly translated into English in the following.

`*** [01] <{t001} --> [opened]>!`

"To make t001 opened!"

`*** [02] <{t001} --> door>.`

"t001 is a door."

`*** [03] <(&/, <(*, {SELF}, {t002}) --> hold>, <(*, {SELF}, {t001}) --> at>, (^open, {SELF}, {t001})) =/> <{t001} --> [opened]>>.`

"If I hold t002, arrive at t001, and execute the operation 'open' at t001, then t001 will be opened."

`*** [04] <(*, {t002}, {t001}) --> key-of>.`

"t002 is the key of t001."

`*** [05] <(&/, <(*, {SELF}, {t002}) --> reachable>, (^pick, {SELF}, {t002})) =/> <(*, {SELF}, {t002}) --> hold>>.`

"If I can reach t002, and execute the operation 'pick' at t002, then I will hold t002."

`*** [06] <(&|, <(*, $x, #y) --> on>, <(*, {SELF}, #y) --> at>) =|> <(*, {SELF}, $x) --> reachable>>.`

"If an object is on something, which is located at the same place as me, I can reach that object."

`*** [07] <(*, {t002}, {t003}) --> on>. :|:`

"t002 is on t003."

`*** [08] <{t003} --> desk>.`

"t003 is a desk."

`*** [09] <(^go-to, {SELF},$x) =/> <(*, {SELF}, $x) --> at>>.`

"If I execute the 'go to' operation aimed at a place, I'll arrive at that place."

The step-by-step process shows that the system uses backward inference to reduce the given goal into derived goals that can be directly achieved by the execution of operations, and uses forward inference to change the description about the situation, according to the expectations on what each operation will achieve, until the initial is achieved.

In this process, the following operation sequence is displayed in Java console, which is what the system actually does:

```
EXECUTE: ^go-to({SELF},{t003})
EXECUTE: ^pick({SELF},{t002})
EXECUTE: ^go-to({SELF},{t001})
EXECUTE: ^open({SELF},{t001})
```

#### Example 2

This example is a variant of the previous one. Here the goal is not achieved by "hiking on the way", but by the forming of a complete plan, which can be executed later when the goal actually appears.

When a plan is derived, it also comes with a truth value, indicating its estimated chance of success, according to the experience of the system.

#### Example 3

This example shows the decision-making process of the system when facing multiple goals, as well as multiple paths to achieve the same goal.

When the system finds that it can simply go to the door and break it, this path will be preferred for its simplicity (therefore higher confidence, everything else being equal). However, when the system realizes that in that way the door will be broken (which violate another goal), then it won't take the action unless it has a sufficiently high motivation (for some other reason) to do that.

#### Example 4

Though in principle all the inference in the previous examples can all be seen as (various types of) learning, this example shows several more typical types of "learning by reasoning" in NARS.

When a belief conflict is judged to be updated, the involved beliefs are not revised. Instead, the old belief is added a past tense, and the system's current opinion is completely determined by the new belief.

When a sequence of events is observed, the system will tentatively build a hypothesis about their relationship, using a special version of induction (and comparison). In this way, the system can gradually acquire skills and causal knowledge from its experience.

### Procedural Learning Example

Though this example is simple, it covers all major aspects of procedural inference, like how to represent knowledge about operations and goals, how to use this type of knowledge, and where is the knowledge coming from.

#### Initial input

The initial given sentences are the following four lines:

`*** [01] <(*, {SELF}, key001) --> hold>!`

This is a goal (indicated by "!"), which ask the statement <(*, {SELF}, key001) --> hold> to be realized. Here Self is a special term indicating the system itself, key001 is the object to be picked up (intuitively, a key), and hold is a relation to be established between the two. Therefore, the goal roughly corresponds to command "Pick up key001!"

`*** [02] (--, <(*, {SELF}, key001) --> hold>). :|:`

This is a judgment (indicated by ".") saying that the key is not holding by the system. Here "--" is the negation connector, and ":|:" indicates present tense.

Truth-values are optional for input judgments. To make things simple, in this example all input judgments are given as binary statements, which means the default truth-value %1.0;0.9% will be assigned to them by the system. Since the internal representation requires higher accuracy than external communication, all the derived judgments will have their truth-values represented explicitly.

```
*** [03] <(&/, <(*, {SELF}, key001) --> reachable>, (^pick, {SELF}, key001)) =/> <(*, {SELF}, key001) --> hold>>.
```

The system's knowledge about the `^pick' operator: "If the operator is executed on key001 after it becomes reachable, it will be held by the system".

In practical situations, such knowledge is usually represented in more general form, with the key001 replaced by independent variable #x, since it applies to many (though not all) terms. However, in that case, the frequency of the belief will not be very high, since there are many things that are reachable, but cannot be picked up. Adding more preconditions will increase the frequency, but decrease the generality and simplicity of the knowledge.

`*** [04] <(*, {SELF}, key001) --> reachable>. :|:`

The key is currently reachable. As shown in other examples in ProceduralExamples, if this is not the case, the system will recursively treat it as a derived goal.

Since this example focuses on the logic of the system rather than the control mechanism, in the following the initial inputs and derived results will be manually selected to feed into the inference engine, with proper timing, so as to lead to the desired behavior, step by step.

#### Decision making

```
********** [03 + 04 -> 05]:
IN: <(&/,<(*,{SELF},key001) --> reachable>,<(*,{SELF},key001) --> ^pick>) =/> <(*,{SELF},key001) --> hold>>.
IN: <(*,{SELF},key001) --> reachable>. :|:
1
OUT: <<(*,{SELF},key001) --> ^pick> =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.81%
```

The first step simplifies the precondition of the operation by removing the conditions that are already realized. Given the uncertainty introduced by this deduction, the conclusion has a lower confidence value. Furthermore, while judgment [03] is tense-free, judgment [05] is true at the current moment, since the condition [04] is not always true.

In [03], operation (^pick, {SELF}, key001) becomes <(*,{SELF}, key001) --> ^pick> when loaded into the system, though the former version remains its standard form in Narsese. In the latter version, the subject can be a product with a single argument, because in all operations there is an implicit argument, the system itself.

```
********** [05 + 01 -> 06]:
IN: <<(*,{SELF},key001) --> ^pick> =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.81%
IN: <(*,{SELF},key001) --> hold>!
1
OUT: <(*,{SELF},key001) --> hold>? :|:
2
OUT: <(*,{SELF},key001) --> ^pick>! %1.00;0.73%
```

When goal [01] is actually pursued, the system first checks if the desired situation has been somehow realized already. If this is the case, then no operation needs to be taken. This "active perception" is triggered by the derived question (indicated by "?"), which also serves as a command to the sensorimotor mechanism to report the perceived truth-value of the statement in the near future.

Also, a command to execute the operation is produced as a derived goal, by backward inference from the given goal and the knowledge about the operation

```
********** [06 -> 07]:
IN: <(*,{SELF},key001) --> ^pick>! %1.00;0.73%
1
OUT: <(*,{SELF},key001) --> ^pick>. :|: %1.00;0.90%
```

Since there is no other reason to prevent this derived goal from being realized, the operation is executed, and this fact is remembered by the system.

#### Expectation and feedback

```
********** [07 + 05 -> 08]:
IN: <(*,{SELF},key001) --> ^pick>. :|:
IN: <<(*,{SELF},key001) --> ^pick> =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.81%
1
OUT: <(*,{SELF},key001) --> hold>. :|: %1.00;0.73%
```

From the fact that an operation has been executed, and knowledge about its consequence, the system forms an expectation about what has become the case. If the knowledge is reliable enough, the expectation will have a reasonably high confidence for the following decisions to be based on

```
********** [02 + 08 -> 08]:
IN: (--,<(*,{SELF},key001) --> hold>). :|:
1
OUT: <(*,{SELF},key001) --> hold>. :\: %0.00;0.90%
1
IN: <(*,{SELF},key001) --> hold>? :|:
1
OUT: <(*,{SELF},key001) --> hold>. :\: %0.00;0.90%
1
IN: <(*,{SELF},key001) --> hold>. :|: %1.00;0.73%
2
IN: <(*,{SELF},key001) --> hold>? :|:
1
OUT: <(*,{SELF},key001) --> hold>. :\: %1.00;0.73%
```

In this sequence of steps, the system is first told that it is not holding the key, then after a short time, it is expecting to be holding the key. Since both judgments have present-tense attached when entering the system, and they are significantly different, the system does not do a revision (that is, to treat them as collecting evidence from different channels about the same situation), but updating (that is, to treat the new judgment as the current situation, and the previous judgment as a situation that was the case, but no longer true).

A present-sense question can be answered by a past-tense judgment, as far as the judgment represents the most recent information about the situation.

```
********** [08 + 09 -> 10]:
IN: <(*,{SELF},key001) --> hold>. :|: %1.00;0.73%
1
IN: <(*,{SELF},key001) --> hold>. :|:
1
OUT: <(*,{SELF},key001) --> hold>. :|: %1.00;0.92%
```

Now the system is getting new input [09], which can be seen as a response triggered by the derived question sent out previously. Since this feedback confirms the expectation, they are merged into a more confident judgment about the current situation.

#### Observation and conditioning

```
********** [07 + 09 -> 11]:
IN: <(*,{SELF},key001) --> ^pick>. :|:
1
IN: <(*,{SELF},key001) --> hold>. :|:
1
OUT: <<(*,{SELF},key001) --> ^pick> =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.45%
```

From the execution of an operation and a following situation, the system derives by induction that the situation is somehow a consequence of the operation.

```
********** [05 + 11 -> 12]:
IN: <<(*,{SELF},key001) --> ^pick> =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.81%
1
IN: <<(*,{SELF},key001) --> ^pick> =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.45%
1
OUT: <<(*,{SELF},key001) --> ^pick> =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.84%
```

The new inductive conjunction confirms a previous belief, so they are merged by the revision rule to become a more stable belief.

```
********** [04 + 12 -> 13]:
IN: <(*,{SELF},key001) --> reachable>. :|:
1
IN: <<(*,{SELF},key001) --> ^pick> =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.84%
1
OUT: <(&/,<(*,{SELF},key001) --> reachable>,<(*,{SELF},key001) --> ^pick>) =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.43%
```

Similarly, a belief about an implication relation can be conditioned, so as to get a more complicated precondition.

```
********** [03 + 13 -> 14]:
IN: <(&/,<(*,{SELF},key001) --> reachable>,<(*,{SELF},key001) --> ^pick>) =/> <(*,{SELF},key001) --> hold>>.
1
IN: <(&/,<(*,{SELF},key001) --> reachable>,<(*,{SELF},key001) --> ^pick>) =/> <(*,{SELF},key001) --> hold>>. :|: %1.00;0.43%
1
OUT: <(&/,<(*,{SELF},key001) --> reachable>,<(*,{SELF},key001) --> ^pick>) =/> <(*,{SELF},key001) --> hold>>. %1.00;0.91%
```

Again, the new evidence is merged with the previous knowledge, to gradually increase the confidence of knowledge about operations.

Also, it can happen, that a future belief does not happen as predicted, in which case anticipation is formed by the system, an application of closed-world assumption justified by the fact that the system expected to observe it but it didn't. To complete the process, revision takes over.

```
IN: <(&/,<a --> b>,+3) =/> <b --> c>>.
IN: <a --> b>. :|:
25
OUT: <(&/,<a --> b>,+3) =/> <b --> c>>. :\: %0.00;0.45%
OUT: <<(&/,<a --> b>,+3) =/> <b --> c>>. :\: %0.92;0.91%
```
