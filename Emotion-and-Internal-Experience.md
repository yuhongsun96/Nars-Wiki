_This aspect of OpenNARS is still under development. Part of the following description has been implemented, but part has not yet._

## Feeling and emotion

Feeling and emotion allow the system to appraise individual items and the overall situation, with respect to the system's goals, so as to make proper responses.

This appraisal process starts from the given (input or implant) goals. The desire-value of these goals represent drives and values imposed on the system by its designer or user. From these given goals, plus the system's beliefs, the derived goals are generated, which is biased by the system's own experience.

Each Goal (as a subclass of Task) has a satisfaction value, which is the quality of the goal's current solution (i.e., the statement that best matches its content), so measures the extent to which the goal has already been satisfied.

There is an overall satisfaction value that measures the extent to which the existing goals have been satisfied, so it measures the system's appraisal of the overall situation. This value is updated after each working cycle by taking a weighted average of its previous value and a new value from the goal processed in the cycle (if there is one) weighted by the priority of the goal. In this way, this satisfaction value represents the appraisal of the system to the "recent" situation.

This appraisal can be felt by the system itself. When the satisfaction is beyond the neutral zone (around 0.5, defined by a system parameter), a "feeling" operator is triggered to represent an event "<{SELF} --> [SATISFIED]>. :|:" as part of the system's internal experience, with the truth-value and priority-value determined by the satisfaction value. In this way, the appraisal enters the system "consciousness," and is processed together with the system's other experienced events. Combined with other information, various feelings can be generated.

Finally, the desire-value in statements will be extended to all concepts. While a statement's desire-value will still be mainly determined by its relations with the goals, for the other concepts their desire-values will be mainly determined by their relation to the overall satisfaction of the system, rather than about the achieving of individual goals. At the end of each working cycle, the desire-value of the fired concept is adjusted by the current satisfaction value (again using a weighted average). So, in the long run, the desire-value of a concept indicates the extent to which its firing is associated with positive emotions of the system, which provides a rough summary with the relation between the concept and the goals.

In summary, at any moment the system's appraisal is represented at two levels: at the "unconscious" (Java) level, it is represented by the desire-values of the concepts, the satisfaction-values of the goals, and the overall satisfaction; at the "conscious" (Narsese) level, it is represented by sentences generated by the related mental operators on what the system "wants" and how it "feels", which selectively express the related information at the unconscious level.

The appraisal information will serve several functions:

* The desire-values of data items (concepts, tasks, and beliefs) will be taken into account by the budget functions, where items with strong feeling (extreme desire-values) will get more resources than items with weak feeling (neutral desire-values).

* The overall satisfaction will be used as feedback to adjust the desire-values of data items, so that the ones associated with positive feeling will be rewarded, and the ones associated with positive feeling punished. In this way, the system will show a "pleasure seeking" tendency, and its extent can be adjusted by a system parameter.

* By involving the feeling-grounded concepts in self-control knowledge, the system will learn strategies to handle various situations specified by their associated emotions. For example, the system will learn what it should do when it is "happy" or "sad", each of which covers many situations that differ in details, but share important common properties.

* The system's feelings and emotions can be communicated to other systems, either by using the "emotional" terms in Narsese, or by showing the related measurements in the GUI.

## Internal experience

Besides satisfaction, the system may use other system-level indicators:

* **busyness** that summarizes the average priority values of the recently processed. This measurement will decide the chance for tentative ideas to be explored.

* **alertness** that summarizes the average difference between recently processed input and the corresponding anticipation. This measurement will decide the time spent on direct input.

* **well-being** that summarizes the situation of energy supply, I/O channel connection, device functioning, etc. This measurement will become necessary when the system directly controls a hardware body.

There will be corresponding feeling operators for each of these indicators. In a sense these indicators are not emotions, since they are not handled exactly as described in the previous section, though in a similar way. All these feelings are included in the system's internal experience.

Beside that, it will also be necessary to record the major events within the system, and make it available to the inference. For this purpose, the current inference log will become partially perceivable by the system itself via certain feeling operations, or, some events will even automatically get into the internal experience, so as to allow the system to answer questions like "What I have been thinking?" or "what methods I've tried on that problem?".

To achieve this purpose, an inference step can be summarized afterward as an _Implication_ statement from the premise(s) to the conclusion. For double-premise inference rules, it may be better to only include one premise (the task) in the summarizing Implication statement, while taking the other one (the belief) as part of the background knowledge.

The internal experience may be treated similarly as the I/O channels, with a buffer and other features, though there is probably only one such channel.