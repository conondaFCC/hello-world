#object oriented program design

## main ideas - repetition
- classes build models for ojbects (real life objects or artificial ones)
- the model describes only 'interesting' properties [or should it say 'relevant' ones]
- the properties can be seperated in:
-- structure [was variables] -> balance and owner of accounts
-- dynamic [was methods] -> topup account, interest

## main ideas 2
tree of life. take biology, the classification of life.
As you go up from the roots, from lifeform untill to the individual name of it, like ants or humans, in between, the branches, that group certain aspects together, like mammals or birds or flora. Or in other words, going up the tree, specialization, going down to the root, generalization.

## main ideas 3
grouped properties can be set once, in a hierarchy!
birds can fly.
the hawk and the condor are birds.
so flying will be set at the birds branch.
this leads to following rule:
the closer to the root, the more general the properties, aka generalization
the closer to the leaf/flower, the more specified the properties, aka specialization

## an example of the main ideas
imagine a superclass the 'File', that defines properties for all files. [with size, name and rename().]
containing the subclass 'Sound' as a deviation/branch of file, inheriting size, name and rename(), and also defining own properties, like coding, play().
and another subclass 'Text' as another deviation/branch of file, also inheriting and definfing its own property, edit().
{XXX replace with or add code segment? depicting text as UML model}

## warning
beginners tend to make unfiting hierarchies. check yours! by saying subclass is a superclass. the owl is a bird, clerk is a job.
especially when shared properties come into play
- carsnivores and herbyvores [XXX this is not a word, right?], can be seperated within mammals and birds.
- solution: references to other classes aka 'compositions' [term in english?]. not the be confounded whit the composition from the uml-class-diagram.

[represented as an image, it would show the following]
instead of the superclass vehicle with the subclasses passenger cars and lorries and eacht of them having two subclasses, like electric passanger cars, gas passenger cars, and electric lorries, gas lorries. imagine, vehicle, subclass is identical, with passenger cars and lorries, but with a composition class 'engine', divided into, electric and fuel driven. [aka the relation discussed in uml-class-diagram].
