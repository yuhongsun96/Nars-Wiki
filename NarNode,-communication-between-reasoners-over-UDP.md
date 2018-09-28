NarNode is used to let two Nar instances communicate over UDP. This allows multiple Nar instances to exchange knowledge over the network, which can be especially useful when multiple Nar instances work on similar problems.
In such cases, the learning speed can be extremely accelerated, because when one Nar agent has to deal with a new case, it might be a case another Nar already had to deal with. This is even more likely when more agents are involved. 

# Creating a NarNode instance
A NarNode instance can be created using **NarNode(int listenPort)** (the port to listen for incoming UDP packets),
alternatively it can be constructed by using an existing Nar instance: **NarNode(Nar nar, int listenPort)**

# How NarNodes communicate
Each NarNode has a list of 
**targets=[targetNar_1,...,targetNar_n]**
where for each **targetNar_i**, tasks above **priorityThreshold(targetNar_i)** are sent to, optionally only if **mustContainTerm(targetNar_i)** is included in the task.
The sent task gets then processed in each **targetNar_i** as if it would be a derived task of itself.

# How to specify NarNodes targets
To add a targetNar to the targets of a NarNode nar, we simply call ****nar.addRedirectionTo**(TargetNar target)**
or alternatively **nar.addRedirectionTo**(String **targetIP**, int **targetPort**, float **taskThreshold**, Term **mustContainTerm**, boolean **sendInput**)
where

**targetIP** = the IP of the target Nar (IPv6 untested, but should work too)

**targetPort** = the target port to connect to

**taskThreshold** = the priority tasks that appear in nar must at least have to be sent to all targets

**mustContainTerm** = a term that must be contained in the task's term as subterm to be sent.