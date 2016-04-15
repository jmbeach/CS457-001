# Functional Dependencies and Normalization - Chapter 14 
## Relation Schema Goodness
+ Logical level - relations and views
+ Storage level - relations as files
+ Placing one set of attributes in a table is better than placing them in other tables.  Why?

## Schema design
+ Design the schema so it is easy to explain the semantics
+ semantics: the meaning associated with the attributes
+ Want to minimize:
	+ storage space
	+ redundant information

## Semantics
+ ![](http://i.imgur.com/PtiFWGR.png)
+ [Figure 15](http://cs457.cs.ua.edu/Figures/Fig14.3.pdf)
+ Do not combine attributes from &gt; 1 entity/relationship type    
Fig 15.3
  
  
Reduce the redundant values
 Design schema so no anomalies occur
 Update anomalies:  insert, delete, update
 
## Update Anomalies
 Insertion
If add employee in department?
if insert new employee into EMP_DEPT and no department yet?   
Fig 15.3
If create a new department and no employee?
Deletion
   If delete last employee of a department?
 Modification
   If change the values of a particular department?
Another example
Emp-Proj
(SSN, 
Pnumber
, Hours, 
Ename
, 
Pname
, 
Plocation
)
6
7
8
Performance
Design schemas so no anomalies occur but what about performance?
Must always do join between employee and department
In general it is best if specify joins as views so anomaly free
If really large tables, may have to rethink this …
Consider:  NoSQL DBs do not have a join
9
Functional Dependencies
How good is a relational schema?
What is the most importance concept in relational schema design?
             Functional Dependencies
Formal concepts and theory to define goodness of relational schemas
Functional dependency FD between 2 sets of attributes as:
                  X → Y
Constraint on the possible tuples that can form a relation instance
10
 Functional Dependencies
 X → Y  means:
X functionally determines Y
Y depends on X
Values of Y component depend on, determined by values of X component
11
Functional Dependencies
Given t1 and t2 where X → Y :
 if t1[X] = t2[X] then t1[Y] = t2[Y]  (1)
In other words if the values of X are equal, then Y values are equal
Values of X component uniquely (functionally) determine values of Y component iff (1)
12
Example
 for example:  address, city → zipcode
ssn → name
if X is a candidate key implies X → Y
if X → Y, does this imply Y → X?
don’t know  -  FD is a property of semantics
dependency is a constraint
 if satisfy FD, instances are legal relation instances (extension)
13
  FDs - set F
describes a relation instance
constraints must hold at all times
property of relation schema not a particular extension
therefore, it cannot be automatically deduced, it must be defined explicitly by designer
14
Normalization to 2
nd
 and 3
rd
 
 Normalization of data - method for analyzing schemas based on FDs
Objectives of normalization
good relation schemas disallowing update anomalies
Unsatisfactory schemas decomposed into smaller ones with desirable properties – 
This means tables are divided up into smaller tables
15
Formal framework
database normalized to any degree (1, 2, 3, 4, 5, etc.)
normalization is not done in isolation
need:
dependency preservation
additional normal forms meet other desirable criteria
lossless join – will discuss later
16
Normal Forms
1
st
, 2
nd
, 3
rd
 consider only FD and key constraints
constraints must not be hard to understand or detect
need not normalize to highest form (e.g. for performance reasons)
17
1NF - 1st normal form
 part of the formal definition of a relation
 disallow multivalued attributes, composite attributes and their combination
 In 1NF single (atomic, indivisible) values
18
Example:
There are 2 ways to look at dnumber → dlocations, where dlocations is more than one value
  
      
dlocations is a set of values
dnumber → dlocations, but dlocations is not in 1NF
dlocations atomic values
dnumber does not functionally determine dlocations
Two different tuples with dnumber=5 can have different values for dlocation= Bellaire or Sugarland or Houston
Another notation
19
20
 How to resolve this?
What are the choices?
Nested relation – Not 1NF
multivalued composite attributes  
research attempts to allow 
and formalize nested relations
Oracle allows it
Normalize it to 1NF
21
Normalize into 1NF
 Algorithm to normalize nested relations into 1NF? 
Replicate tuple for each set value
New PK:  PK and set-valued attribute
22
Normalize into 1NF
Can do the same to normalize 
nested tables to 1NF
Replicate tuple for row in nested table
 New PK:  PK and key of nested table
  recursively 
unnest
 if multilevel nesting
  useful in converting hierarchical schemes into 1NF
23
24
 Difficulties with 1NF
 insert, delete, update
 Determine if describe entity identified by PK?
 If not, called non-full FDs
 We need full FDs for good inserts, deletes, updates
25
 Second Normal Form - 2NF
 Uses the concepts of FDs, PKs and this definition:
  An FD is a Full functional dependency if:
               given Y → Z 
Removal of any attribute from Y means the FD does not hold any more
Obviously Y would be more than 1 column
26
2NF – Partial Dependency
Examples:  
Fig. 15.11
      {ssn, pnumber} → hours     
	is a full FD since neither
 ssn → hours nor pnumber → hours holds
Partial Dependency
 {ssn, pnumber} → ename is not a full FD 
       it is a partial dependency since
ssn → ename also holds
27
2NF
A relation schema R is in 2NF if:
Relation is in 1NF
Every non-prime attribute A in R is 
not
 partially dependent on any key (primary or candidate)
Definition:  Prime attribute - attribute that is a member of the primary key K, so non-prime not in the PK
In other words – 
No partial dependencies
28
Remove partial dependencies: How?
Solution
R can be 
decomposed
 into 2NF relations via the process of 2NF normalization
Remove partial dependencies by:  How?
From original table, remove attribute(s) that is partially dependent and place in a new table
Replicate the part of the primary key on which there is the partial dependency and put in the new table
Result is 2 relations where partials are now full
29
30
31
2NF – Formal definition
The above definition considers the primary key only (which is &gt; 1 column)
The following more general definition takes into account relations with multiple candidate keys
A relation schema R is in 2NF if every non-prime attribute A in R is not partially dependent on any key (
including candidate keys of R
) 
County_name and lot# are candidate keys
32
33
    2NF problems:
Even if no partial dependencies problems with insert, delete, modify
Why?
Transitive dependencies
Given a set of attributes Z, where Z is not a subset of any key and
X is a key 
Both X → Z and Z → Y 
then we have a transitive dependency 
{X → Y, Y → Z} |= X → Z
34
     Examples of Transitive FDs
 Examples: 
Fig 15.11
 
      ssn → dmgrssn is a transitive FD 
	since
      ssn → dnumber and dnumber → dmgrssn
    Also,  
 	   ssn → dnumber and dnumber → dname
   ssn → ename is non-transitive 
           since 
     there is no set of attributes X
           where ssn → x and x → ename
 
35
36
 3rd Normal Form (3NF)
 No non-prime attribute is transitively dependent on a primary key and the table is in 2NF
 intuitively, this means we need independent entity facts steps for normalization
 disallow partial and transitive dependency on primary/candidate keys
37
3NF 
A relation schema R is in 3NF if:
it is in 2NF
no non-prime attribute A in R is transitively dependent on the primary/candidate key
In other words – 
no transitive dependencies
R can be decomposed into 3NF relations via the process of 3NF normalization
Which is?
38
39
3NF
Formal Definition:
 a superkey of relation schema R - a set of attributes S of R that contains a key of R
A relation schema R is in 3NF if whenever 
		X  -&gt; A  holds in R
then either
   a)  X is a superkey of R
		or
   b)  A is a prime attribute of R
    a) means every non-prime attribute is fully functionally dependent on every key
    b) means no transitive dependencies on any key 
40
41
42
43
RecruiterID,City, State → NoOfRecruits
RecruiterID → RecruiterName
RecruiterID → StatusID
RecruiterID → Status
StatusID → Status
City, state → CityPopulation
State → StatePopulation
Alternative notation
44
45
To Normalize (3NF)
Consider primary and all candidate keys
Make sure in 1
st
If any partial dependencies, normalize to 2
nd
If transitive dependencies, normalize to 3rd
46
47
Normal forms:
Each normal form is strictly stronger than the previous one:
 every 2NF relation is in 1NF
 every 3NF relation is in 2NF
Armstrong’s Axioms
IR3: {X → Y, Y → Z} |= X → Z
IR1: If X ⊇ Y, then X → Y
48
Additional normal forms:
 BCNF – 3.5 Normal Form	
Stronger than 3NF
A relation schema R is in BCNF if whenever 
		X  -&gt; A  holds in R
then either
   a) X is a 
superkey
 of R
		or
   b) 
X 
⊇
 A
49
50
Your pizza can have exactly three topping types:
one type of cheese
one type of meat
one type of vegetable
Pizza can have toppings, that are of topping type
PK: Pizza, Topping → Topping Type
CK: Pizza, Topping type → Topping
Topping → Topping Type
Note the Multiple overlapping candidate keys
51
Pizza    Topping    Topping Type
-------- ---------- 	-------------
1        mozarella  	cheese
1        pepperoni  	meat
1        olives     	vegetable
2        mozarella  	meat
2        sausage    	cheese
2        peppers    	vegetable
Wait a second, mozarella can't be both a cheese and a meat! And sausage isn't a cheese!
The problem is Topping → Topping Type satisfies, 3NF
Needs BCNF, Topping is not a superkey or an improper superclass of Topping Type
PK: Pizza, Topping → Topping Type
CK: Pizza, Topping type → Topping
Topping → Topping Type  //Does not violate 3NF
52
Pizza    Topping
-------- ----------
1        mozarella
1        pepperoni
1        olives
2        mozarella 
2        sausage
2        peppers
Topping    Topping Type
---------- -------------
mozarella  cheese
pepperoni  meat
olives     vegetable
sausage    meat
peppers    vegetable
So how to decompose?
Take out Topping Type and create a new table
53
54
Additional normal forms:
4NF - based on multi-valued dependencies
No table may contain more than 1 multivalued relationship 
	
Interesting example:
http://en.wikipedia.org/wiki/Fourth_normal_form
     
States 20% of tables in organizational DBs that were studied violated 4NF
Additional aspects
Determine if one set of FDs covers another
Determine if one set of FDs is equivalent to another
See if they cover each other
Determine if a minimal cover for a given set of dependencies
How to do this?
55
Armstrong’s Axioms
IR1: If X ⊇ Y, then X → Y
IR2: {X → Y} |=XZ → YZ
IR3: {X → Y, Y → Z} |= X → Z
IR4: {X → ZY} |= X → Y
IR5: {X → Y, X → Z} |= X → YZ
IR6: {X → Y, WY → Z} |= WX → Z
56
57
    Decomposition
 Relational database schema design is synthesis and decomposition
 synthesis - grouping attributes together
 decomposition - avoiding transitive and partial dependencies
strict decomposition - start with a universal relation
          OR
 
ER model mapped to a set of relations using the rules
Maps to 3NF 
58
Additional Design Considerations - Reduce nulls
 Avoid placing attributes in a base relation whose values may be null for a majority of tuples
If use null values can mean different things
"fat" tuples - if many attributes and lots of nulls wastes space
Aggregate functions are  a problem with nulls
59
Disallow spurious tuples
Spurious tuples represent incorrect information that is not valid
Result of joins with equality conditions on attributes that are not PKs or FKs
Design relations so there can be an equijoin with a PK and a FK or no spurious tuples 
Lossless join
 guarantees no spurious tuples
         
60
select * from emp_locs l, emp_proj1 p
Where l.plocation=p.plocation
61
Good design  
The goal is to have each relation in 3NF
 Semantics should be clear
  Reduce the redundant values
  Reduce null values
  Disallow spurious tuples
62
Good design 
 A "good" design is not simple individual relations in a higher normal form
also a set of relations with characteristics such as:
 attribute preservation - each attribute appears once (at least)
 dependency preservation - each dependency is a constraint to enforce a join
(
S
 T U V)   S-&gt;T S-&gt;V  T-&gt;U 
is (
S
 V) (
T
 U) a good decomposition?
union of dependencies holds - does not guarantee a lossless join
But?
Performance  vs. normalization
Denormalization
 – may have to do this
   useful concept in NoSQL
63
