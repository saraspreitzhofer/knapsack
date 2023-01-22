# The knapsack problem: Greedy and Genetic Algorithms

This notebook includes a greedy algorithm, a genetic algorithm based on the pyeasyga framework and an improved genetic algorithm for the one-, multi- and multiple multidimensional knapsack problem.


## Introduction & Goals

In general, the knapsack problem is a mathematical combinatorial problem with the goal of packing items into a knapsack to obtain the highest profit possible. The knapsack has a capacity c which cannot be exceeded [S. Martello and P. Toth, Knapsack Problems: Algorithms and Computer Implementations. England: John Wiley & Sons, Inc., 1990.]. Due to its ongoing importance in many applications, for example in the capital budgeting problem and the container loading problem, the knapsack problem is of high value in research. However, an optimal solution to the problem is yet to be found.

This notebook includes a greedy algorithm, a genetic algorithm based on the pyeasyga framework and an improved genetic algorithm for the one-, multi- (MKP) and multiple multidimensional knapsack problem (MMKP). The goal of this project is to find out how the simple genetic algorithm can be improved to solve different knapsack problem variants, and if this modified genetic algorithm, the simple genetic algorithm or the greedy algorithm performs best and why. 

The source code is available at:

GitHub Repository: https://github.com/saraspreitzhofer/knapsack_greedy_genetic 

Google Colab Notebook: https://colab.research.google.com/drive/1XCYEeYtcj7IbkQMUJzPvR5OwTlIC0ctl?usp=sharing 

## Technologies & Setup

Algorithms are developed with the help of the pyeasyga framework. To import this framework, the command “pip install pyeasyga” is used. As development environment for Python 3 code both PyCharm Professional and Google Colaboratory with a Jupyter notebook are used.

For the initialization of the knapsack and the items, the benchmark instances used in [J. Puchinger, G. Raidl, and U. Pferschy, “The multidimensional knapsack problem: Structure and algorithms,” INFORMS Journal on Computing, vol. 22, pp. 250–265, 2010.] were taken as a reference point. Concerning the knapsack itself, it has a maximum capacity of 50 per dimension. To fill the knapsack, a total number of 250 items is created, each with one value and one (for the one-dimensional knapsack) or ten constraint(s) (for the MKP). The items are internally saved as a dictionary datatype. Each of these items gets its profit and the one or ten resource consumption value(s) randomly assigned, where the profit is an integer number between one and 100, and the resource consumption values lie between one and 15. The MMKP consists of five knapsacks, each having the same properties as used for the MKP. 

## Structure

The notebook is divided into the following sections:
1.	Initialization
2.	Greedy Algorithm, 1-dimensional 0-1 Knapsack
3.	Greedy Algorithm, 2-dimensional 0-1 Knapsack
4.	Greedy Algorithm, n-dimensional 0-1 Knapsack
5.	Greedy Algorithm, multiple n-dimensional 0-1 Knapsack
6.	Genetic Algorithm, 1-dimensional 0-1 Knapsack with PYEASYGA
7.	Genetic Algorithm, n-dimensional 0-1 Knapsack with PYEASYGA
8.	Improved PYEASYGA module
9.	Improved Genetic Algorithm, 1-dimensional 0-1 Knapsack
10.	Improved Genetic Algorithm, n-dimensional 0-1 Knapsack
11.	PYEASYGA module for multiple Knapsack
12.	Genetic Algorithm, multiple n-dimensional 0-1 Knapsack with PYEASYGA
13.	Improved PYEASYGA module for multiple Knapsack
14.	Improved Genetic Algorithm, multiple n-dimensional 0-1 Knapsack

## Implementation

### Greedy Algorithm 

The greedy algorithm is a simple and efficient algorithm for optimization problems. To solve a problem, it is divided into subproblems, and the algorithm chooses the locally optimal solution for each subproblem. Once a choice is made, it cannot be changed afterwards [T. H. Cormen, C. E. Leiserson, R. L. Rivest, and C. Stein, Introduction to Algorithms, 3rd ed. Massachusetts: Massachusetts Institute of Technology, 2009.]. In the knapsack problem, each subproblem of the algorithm is the choice of which item has to be packed into the knapsack next. To determine the locally optimal solution, all items are sorted according to the highest profit density heuristic, meaning the ratio of value over weight. For the MKP, the used heuristic is the ratio of the value over the sum of all other dimensions. 

In the MMKP, the knapsacks are filled one after the other, according to the greedy principle. First, the available items are sorted with the heuristic from the MKP. Based on this sorted list of items, the first knapsack is filled by taking item after item as long as no constraint is violated. Subsequently, all unused items are used as the new sorted list for the next knapsack. This procedure continues until all knapsacks are filled.

### Genetic Algorithm

Genetic Algorithms (GAs) are a class of evolutionary algorithms for optimization, inspired by biology, more precisely by the natural selection and evolution of species [X. Yu and M. Gen, Introduction to Evolutionary Algorithms. London: Springer-Verlag London Limited, 2010.]. To solve a problem, the GA looks at a population, which is defined as an initial set of random possible solutions. Each of these solutions is called individual or chromosome [P. Guo, X. Wang, and Y. Han, “The enhanced genetic algorithms for the optimization design,” in 2010 3rd International Conference on Biomedical Engineering and Informatics, vol. 7, 2010, pp. 2990–2994.]. An individual in the case of the knapsack problem is a sequence of 1s (item is selected) and 0s (item is not selected), called the binary vector x. A population is then a set of different vectors x_i.

In each iteration, also called generation, the GA evaluates each individual using a fitness function [P. Guo, X. Wang, and Y. Han, “The enhanced genetic algorithms for the optimization design,” in 2010 3rd International Conference on Biomedical Engineering and Informatics, vol. 7, 2010, pp. 2990–2994.]. Therefore, a selection function is implemented, which selects two parents from the current generation, determines the one with the higher fitness value and adds it to the new generation, until the population size is reached. This process is called tournament selection with a tournament size of two [P. C. Chu and J. E. Beasley, “A genetic algorithm for the multidimensional knapsack problem,” Journal of Heuristics, vol. 4, no. 1, p. 63–86, jun 1998.].

Individuals evolve over multiple generations. Thus, the GA implements crossover and mutation operators which are applied at a certain probability to the selected individuals from the tournament selection. Crossover means that two individuals are split at a random index. Then, the first part of the first individual is merged with the second part of the second individual to produce the first child, and the first part of the second individual is merged with the second part of the first individual to produce the second child. This so-called offspring is then part of the next generation. With mutation, random bits from specific individuals are modified [P. Guo, X. Wang, and Y. Han, “The enhanced genetic algorithms for the optimization design,” in 2010 3rd International Conference on Biomedical Engineering and Informatics, vol. 7, 2010, pp. 2990–2994.], for example single bits from the vectors x_i are flipped.

To approximate the optimal solution, the GA generates multiple generations containing a constant number of individuals. Each new generation consists of some parents and offspring. This procedure continues until the stop criteria is fulfilled [X. Yu and M. Gen, Introduction to Evolutionary Algorithms. London: Springer-Verlag London Limited, 2010.], which in this project is if a predefined number of generations “maxgen” is reached.

The pyeasyga MKP example from [A. Remi-Omosowon. Examples. (accessed: 21.10.2022). [Online]. Available: https: //pyeasyga.readthedocs.io/en/latest/examples.html] was taken as a model for the implementation. The pyeasyga framework provides functionalities to initialize and run the genetic algorithm with either default or separately configurable parameter values. It automatically makes use of crossover and mutation for creating an offspring at a default or given probability. The fitness function in this project returns the maximum profit of a given individual. If any dimension exceeds the predefined maximum capacity, the individual gets assigned a profit of zero, so that it is not taken into account for the next generation. The hyperparameters used in this project are 75 generations, a population size of 150, a crossover probability of 0.8 and a mutation probability of 0.2.

For the implementation of the MMKP, several adjustments to the GA for the MKP are necessary. On the one hand, the “fitness” function for the MMKP now calculates the maximum profit as sum of the profits of all knapsacks. On the other hand, the class “GeneticAlgorithm” from the pyeasyga framework has to be modified. In the initialization, the variable “knapsacks” is added, which has a default value of 1 to cover the MKP as well. In addition, three functions of this class have to be adapted. The function “create_individual” has to return a list of length knapsacks”, containing random lists of length “len(seed_data)” filled with 0s and 1s, instead of only one random list of 0s and 1s of length “len(seed_data)” as used for the MKP. Secondly, the “crossover” function for the MMKP performs one crossover at random indices for each single knapsack. Finally, the “mutate” function performs one mutation for each single knapsack, again at random indices for each knapsack.

In order to assure that within one individual, each item is only assigned to a single knapsack or to no knapsack at all, the three functions mentioned above have to be extended even more. After having created an individual in “create_individual”, the function looks for the first 1 of each item index for all knapsacks. In other words, there is an outer for-loop in the range of the number of items, and an inner for-loop in the range of the number of knapsacks. If a 1 is found at a specific index, all items at the same index from the other knapsacks are set to 0. Additionally, the “crossover” function implements the same mechanism. After the crossover of two parents, each of the two children is checked and potentially repaired with the same nested for-loop. Moreover, after a mutation from 0 to 1, the “mutate” function loops over all other knapsacks and sets the value at the mutation index 0. All of this ensures that no item is used more than once.

### Improved Genetic Algorithm

To improve the performance of the GA, an improved GA was implemented. This improved algorithm is based on a pre-optimized starting population, as proposed in [G. Raidl, “An improved genetic algorithm for the multiconstrained 0-1 knapsack problem,” in 1998 IEEE International Conference on Evolutionary Computation Proceedings. IEEE World Congress on Computational Intelligence (Cat. No.98TH8360), 1998, pp. 207–211.]. More precisely, only feasible individuals are generated for the initial population. For the knapsack problem, the class “GeneticAlgorithm” from the pyeasyga framework was changed in two places. Firstly, the two class variables “max_capacity” and “number_of_dimensions” are added to the constructor in order to enable the algorithm to access these knapsack parameters. The “number_of_dimensions” variable has a default value of 1, so that it does not have to be specified as an input parameter in the GA for the one-dimensional knapsack problem.

Secondly, the function “create_individual” is overridden to first validate the feasibility of the created individual before returning it as candidate solution. In detail, instead of just returning a random list of 0s and 1s of the length of the input data, the list is filled iteratively, but still randomly. After each step, the used capacity is compared to the maximum capacity. If the maximum capacity is exceeded, the current position and all following positions of the list are filled with 0s. Consequently, only feasible possible solutions are returned.

Additionally, especially if the starting solutions found are very similar, there is the risk of only finding local optima instead of global ones. To ensure a high population diversity a random permutation of the input data before determining a random individual, which leads to many different starting solutions [G. Raidl, “An improved genetic algorithm for the multiconstrained 0-1 knapsack problem,” in 1998 IEEE International Conference on Evolutionary Computation Proceedings. IEEE World Congress on Computational Intelligence (Cat. No.98TH8360), 1998, pp. 207–211.]. The “random.shuffle” function is used at the very beginning of the “create_individual” function to achieve the permutation. The parameter of this function has to be a list of items “list(items)” instead of just “items”, since the datatype of “items” is a dictionary, and “random.shuffle” cannot work with dictionaries. The parameter values of the improved GA are exactly the same as the ones selected for the GA, since they are the most optimal ones and to be able to compare the simple to the improved GA. 

The improved GA for the MMKP uses the same method of improvement, the pre-optimization of the starting population. The original genetic algorithm for the MMKP is modified at the same positions as the original genetic algorithm for the MKP to reach the same kind of improvement. The function “create_individual” creates the individual iteratively, item after item, and knapsack after knapsack. If one knapsack reaches its maximum capacity, the remaining knapsack is filled with 0s, and the algorithm moves on to the next knapsack. After having created a feasible solution, it still has to be ensured that each item is only assigned to a single knapsack, which is achieved in a similar way as it is done for the original genetic algorithm for the MMKP. All in all, the algorithm can be seen as the algorithm for the MKP, executed as often as there are knapsacks.

## Results & Conclusion

When comparing the best results from both the greedy and the genetic algorithm, it is obvious that the greedy algorithm performs much better than the simple GA concerning runtime and maximum profit, for all one-, multi- and multiple multidimensional knapsack problems. Therefore, the improved GA was implemented. The direct comparison of the results of the three algorithms for the three knapsack problem variants is shown in the table. Additionally, the graph visualizes the results for a better overview. It shows the runtime in milliseconds on the x-axis and the maximum profit on the y-axis. Results from the same knapsack variant are displayed in the same color and are connected through a line.

 
![results table](https://github.com/saraspreitzhofer/knapsack_greedy_genetic/blob/main/results_table.png?raw=true)

![results graph](https://github.com/saraspreitzhofer/knapsack_greedy_genetic/blob/main/results_graph.png?raw=true)
 

Especially the extremely short runtime of the greedy algorithm outperforms both other algorithms. This can also be observed on the graph, since the greedy algorithm is always the one on the left hand side for each color. However, this is no surprise, since the fast speed can be directly deduced from the greedy principle. Moreover, the more complex the knapsack problem gets, the longer the runtime gets, independent of the type of algorithm, which was also an expected result. Therefore, the orange line spreads the furthest to the right in the graph.

The improved GA does indeed reach better results than the simple GA, not only a higher maximum profit, but also shorter runtimes for all problem variants. In the graph, the points for the improved GA always have lower x-values and higher y-values than the points from the simple GA. Particularly for larger input sizes, the importance of this improvement will rise even more.

Another observation is that the difference in performance between the greedy algorithm and the improved GA declines with rising problem complexity. While there is a relatively large gap between the maximum profit for the one-dimensional greedy algorithm and the one-dimensional improved GA, the gap between the multiple multidimensional problem for both algorithms is remarkably smaller. More precisely, the maximum profit of the one-dimensional improved GA is 63.76% of the maximum profit of the one-dimensional greedy algorithm. For the multiple multidimensional problem, however, it is 87.38%, which is a huge increase. In other words, the orange slope from the multiple multidimensional greedy algorithm result to the improved GA result is much lower than the blue slope from the one-dimensional greedy algorithm result to the improved GA result.

To conclude, it can be said that the improvement of the GA with the pre-optimized starting population indeed leads to better results in performance and runtime compared to the simple GA. Additionally, the performance of the improved GA improves with more complex problems. However, the greedy algorithm is the fastest and should therefore be used for real-time applications or in general for applications where a fast runtime is a critical factor. For problems where the runtime of the algorithm is not the most important criteria, the improved GA is a promising choice to reach a high maximum profit, even for very complex problems with large input sizes and many variables and constraints.
