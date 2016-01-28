
FyLearn
=======

FyLearn is a fuzzy machine learning library, built on top of [SciKit-Learn](http://scikit-learn.org/).

SciKit-Learn contains many common machine learning algorithms, and is a good place to start if you want to play or program anything related to machine learning in Python. FyLearn is not intended to be a replacement for SciKit-Learn (in fact FyLearn depends on SciKit-Learn), but to provide an extra set of machine learning algorithms from the fuzzy logic community.

Machine learning algorithms
---------------------------

 - fylearn.frr.FuzzyReductionRuleClassifier
 - fylearn.frrga.FuzzyPatternClassifierGA
 - fylearn.frrga.FuzzyPatternClassifierLocalGA
 - fylearn.fpt.FuzzyPatternTreeClassifier
 - fylearn.fpt.FuzzyPatternTreeTopDownClassifier
 - fylearn.garules.MultimodalEvolutionaryClassifer
 - fylearn.nfpc.FuzzyPatternClassifier


### Usage

You can use the classifiers as any other SciKit-Learn classifier:

    from fylearn.nfpc import FuzzyPatternClassifier
    from fylearn.garules import MultimodalEvolutionaryClassifier
    from fylearn.fpt import FuzzyPatternTreeTopDownClassifier

    C = (FuzzyPatternClassifier(),
         MultimodalEvolutionaryClassifier(),
         FuzzyPatternTreeTopDownClassifier())

    for c in C:
        print c.fit(X, y).predict([1, 2, 3, 4])

Heuristic search methods
------------------------

Several heuristic search methods are implemented. These are used in the learning algorithms
for parameter assignment, but, are also usable directly.

 - fylearn.local_search.PatternSearch
 - fylearn.local_search.LocalUnimodalSampling
 - fylearn.ga.GeneticAlgorithm: Search parameters using modification and a scaling
 - fylearn.ga.UnitIntervalGeneticAlgorithm: Search parameters in unit interval universe.
 - fylearn.ga.DiscreteGeneticAlgorithm: Search parameters from discrete universe.

Example use:

    import numpy as np
    from fylearn.ga import UnitIntervalGeneticAlgorithm, helper_fitness, helper_n_generations
    from fylearn.local_search import LocalUnimodalSamplingOptimizer, helper_num_runs

    def fitness(x):  # defined for a single chromosome, so we need helper_fitness for GA
        return np.var(x)

    ga = UnitIntervalGeneticAlgorithm(fitness_function=helper_fitness(fitness), n_chromosomes=100, n_genes=10)
    ga = helper_n_generations(ga, 100)
    best_chromosomes, best_fitness = ga.best(1)
    print "GA solution", best_chromosomes[0], "fitness", best_fitness[0]

    lower_bounds, upper_bounds = np.zeros(10), np.ones(10)
    lus = LocalUnimodalSamplingOptimizer(fitness, lower_bounds, upper_bounds)
    best_solution, best_fitness = helper_num_runs(lus, 100)
    print "LUS solution", best_solution, "fitness", best_fitness

At tiny fuzzy logic library
---------------------------

Tiny, but hopefully useful. The focus of the library is on providing membership functions and aggregations that work with NumPy, for using in the implemented learning algorithms.

### Membership functions

 - fylearn.fuzzylogic.TriangularSet
 - fylearn.fuzzylogic.TrapezoidalSet
 - fylearn.fuzzylogic.PiSet
 
Example use:

    import numpy as np
    from fylearn.fuzzylogic import TriangularSet
    t = TriangularSet(1.0, 4.0, 5.0)
    print t(3)   # use with singletons
    print t(np.array([[1, 2, 3], [4, 5, 6]]))  # use with arrays

### Aggregation functions

Here focus has been on providing aggregation functions that support aggregation along a specified axis for 2-dimensional matrices.

Example use:

    import numpy as np
    from fylearn.fuzzylogic import meowa, OWA
    a = OWA([1.0, 0.0, 0.0])  # pure AND in OWA
    X = np.random.rand(5, 3)
    print a(X)  # AND row-wise
    a = meowa(5, 0.2)  # OR, andness = 0.2
    print a(X.T)  # works column-wise, so apply to transposed X

Installation
------------

You can add FyLearn to your project by using pip:

    pip install -e git+https://github.com/sorend/fylearn.git#egg=FyLearn-master

To Do
-----

We are working on adding the following algorithms:

 - ANFIS.
 - FRBCS.

About
-----

FyLearn is supposed to mean "FuzzY learning", but in Danish the word "fy" means loosely translated "for shame". It has been created by the Department of Computer Science at Sri Venkateswara University, Tirupati, INDIA by a [PhD student](http://www.cs.svuni.in/~sorend/) as part of his research.

Contributions:
--------------

 - fylearn.local_search Python code by [M. E. H. Pedersen](http://hvass-labs.org/) (M. E. H. Pedersen, *Tuning and Simplifying Heuristical Optimization*, PhD Thesis, University of Southampton, U.K., 2010)

