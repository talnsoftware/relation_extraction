# relation_extraction

Description of the resources

------------------------
1-SSynt-DSynt_PREProc1
------------------------
- map all verbal tags to dpos = VB
- map all nominal tags to pos = NN
- create "lex" attribute by merging predName and predValue CoNLL columns, so we can access lexicon entries (NN and VB)
	- + fallback if no "predValue"
	- + different handling of NNP
	- + equivalent rule for non NN/VB
- mark nominal and verbal arguments with the "lex" of their governor, in order to facilitate the access to lexicon
- mark verbs involved in passive
- mark genitive constructions for facilitating further processing
- mark possessive constructions for facilitating further processing

------------------------
2-SSynt-DSynt_PREProc2
------------------------
- mark all nodes to be removed (mark all hypernode configurations)
- mark all conjuncts of a coordination with the coordination conjunction found at the bottom

------------------------
3-SSynt-DSynt_PTB
------------------------
- transfer all nodes and hypernodes
- map relations to DSyntRels
	- fix some SSynt ill-formedness during the process
	- special rules for genitives and possessives
-  map hypernode configurations to attribute/values when needed
	- aspect, voice, modality, sent_type
- special rules for ID marking
- creates sentence type attribute (declarative, interrogative, etc.)
- mark relative clause constructions (hasRelPro, hasAntecedent)
- mark circumstancials (map SSyntRels to attributes that match semanticon entries)
- mark duplicate DepRels of siblings (needed for post-processing)

------------------------
4-DSynt-POSTProc
------------------------
- remove "it" when another DSyntArg I is dependent of the verb
- resolve argument duplications
- special rule for OPRD constructions without sibling II
- add nodes in gapped coordinations
	- also establish "correct" relations between the introduced node and the original conjuncts (based on GAP- rel)
- mark predicates with external argument information
- mark conjuncts in a coordination with conjunct number (up to 30 conjuncts)
- mark root

------------------------
5-DSynt-Sem
------------------------
- special rules for recovering arguments in control/raise constructions
- special rules for coordinations (conjunction is the head in Sem, and receives external relations)
- special rule that introduces a conjuntion if there is no conjunction in a coordination
- special rule for marking circumstancials/elaborations shared by coordinated elements
- special rules for semantemes and elaborations (new node created)
- special rules for relatives without relative pronouns
- rules for getting VerbNet classes in case they are not provided in the input
	- mark backup VN class if the current class is not found in the VN lexicon
- mark binary/unary predicates

------------------------
6-Sem-POSTProc1
------------------------
- special rules for recovering shared arguments in coordinations
	- in combination with elaboration/semanteme nodes or not
- rules for marking light "be" constructions, when we want to remove the "be"

------------------------
7-Sem-POSTProc2
------------------------
- rules for removing "be" nodes when purely predicative and connecting the remaining nodes
	- also in combination with different kinds of coordinations
- special rules to transfer_attributes from "be" to conjunct(s)
- mark FN frames
	- straightforward, membership, with backup VN class
	- special rule for prepositions, adjectives and determiners: also look in the adverb member list

------------------------
8-Sem-VerbNet
------------------------
- looks for VerbNet classes to assign to nodes
- maps arguments according to dictionary
	- straightforward, default, and with VN backup class
- default mappings for unary and binary predicates
- default mappings for semantemes and elaborations
- mark Frame Elements in order for MATE to only maintain ONE FE if there are several in the lexicon (MATE overwrites attributes, but not relations or nodes)

------------------------
9-VerbNet-FrameNet
------------------------
- special mapping of nodes coming from semantemes (as typed non-core)
- special mapping of nodes coming from simple prepositions as untyped non-core
- special mapping of arguments of unary predicates as Event/Entity/Arg1
- check connectedness (go through the graph starting from the root marked at the SSynt level)
