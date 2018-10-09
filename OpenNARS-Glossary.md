**C**
<br/><br/>
**[Copula](https://github.com/opennars/opennars/wiki/Narsese-Grammar,-Language-of-OpenNARS)** - a relational operator used within statement to indicate relationship between terms
<br/><br/>
**Confidence** - measurement of degree of believe in range [0,1], given __k__  as a temporal constant, might be seen as number of steps in the future, confidence __c__ is defined in terms of evidence w as **__c = w / (w + k)__**, default value for __k__ is 1
<br/><br/>
**D**
<br/><br/>
**detachment** - TODO
<br/><br/>
**Durability** - measurement of computational resources allocated in priority data structure, it is attached to the concept and also serves as a time decay similar to human memory that tends to forget things
<br/><br/>
**E**
<br/><br/>
**Evidence** - given the statement **S-->P**, evidence is in the intersection of _extension _of **S** **and** _intension_ of **P**
<br/><br/>
**Evidence positive** - given the statement **S-->P**, positive evidence is intersection of _extensions_ of both **S** and **P** **and** intersection of _intensions_ of both **P** and **S**
<br/><br/>
**Evidence negative** - given the statement **S-->P**, negative evidence is difference of _extensions_ of both **S** and **P** and difference of _intensions_ of both **P** and **S**
<br/><br/>
**Evidence total** - total evidence is the **sum** of negative and positive evidences
<br/><br/>
**Extension** - extension of a term is a set of terms that appears to the LEFT of inheritance copula (x-->T, where x is extension of t)
<br/><br/>
**Extensional Set** - 
<br/><br/>
**F**
<br/><br/>
**Frequency** - one of key metrics in NARS, it is measurement of uncertainty in the range **[0,1]** and defined as __positive evidence / total evidence__
**I**
<br/><br/>
**Intension** - intension of a term is a set of terms that appears to the RIGHT of inheritance copula (T-->x where x is intension of T)
<br/><br/>
**Intensional set** - 
<br/><br/>
**N**
<br/><br/>
**[NAL](https://github.com/opennars/opennars/wiki/Non-Axiomatic-Logic-(NAL),-Logic-behind-OpenNARS)** - Non-Axiomatic Logic core logic used in OpenNARS <br />
**[Narsese](https://github.com/opennars/opennars/wiki/Narsese-Grammar,-Language-of-OpenNARS)** - Grammar Statement used for communication with OpenNARS control mechanism
<br /><br />

**P**
<br /><br/>
**[Predicate term](https://github.com/opennars/opennars/wiki/Narsese-Grammar,-Language-of-OpenNARS)** - term used to the right of [copula](https://github.com/opennars/opennars/wiki/Narsese-Grammar,-Language-of-OpenNARS)
<br /><br/>

**S**
<br /><br/>
**Statement** - Connection of two terms, subject and predicate, with a copula.
<br /><br/>
**[Subject term](https://github.com/opennars/opennars/wiki/Narsese-Grammar,-Language-of-OpenNARS)** - term used to the left of [copula](https://github.com/opennars/opennars/wiki/Narsese-Grammar,-Language-of-OpenNARS)
<br /><br/>


**T**
<br /><br/>
**Term** - simplest atomic structure within a grammar statement, basically a string of letters in alphabet
<br/><br/>
**Transitive Closure** - given that inheritance (-->) relation is transitive, that is if A-->B and B-->C then A-->C, closure is a set of statements generated by closure-algorithm using transitivity property  
<br/>
**Truth value** - each task is assigned a truth value, that is a pair of real numbers (frequency and confidence) in the range [0, 1]