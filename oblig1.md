# Assignment 1 IN3050
##### Sophus Bredesen Gullbekk



## 1. Problem

We are tasked with solving the traveling salesman problem (TSP), using three different algorithms. We are given a data file containing 24 European cities and the distance between them. The challenge is to find the most efficient path that visits all the cities exactly once. 
<!-- 
All the code used to generate the solutions are in the jupyter notebook.  -->

## 2. Exhaustive Search

The first algorithm we implement to solve TSP is exhaustive search. There we solve the problem by checking every possible tour, and see which one produced the best result. 

We observe that TSP is a cyclical problem, meaning that several permutations of solutions is identical with different starts. Therefore we can remove the last city, calculate the possible permutations without that city and append the last city to the back of all the permutations. This way we get a complexity of 
$\mathcal{O}((n-1)!)$, where $n$ is the number of cities, which will speed up the calculations significantly. 

We calculate the results and the time taken for between 6 and 10 cities and get the following output: 

```console
time to run with 6 cities  is: 0.0004 seconds
time to run with 7 cities  is: 0.0022 seconds
time to run with 8 cities  is: 0.0155 seconds
time to run with 9 cities  is: 0.1240 seconds
time to run with 10 cities is: 1.0932 seconds
```

We also plot the resulting route for both 6 and 10 cities. They are given in figure 1 and 2 below. 

Among the first 10 cities in our data, the shortest tour is:
```console
['Dublin', 'Brussels', 'Hamburg', 'Copenhagen', 
'Berlin', 'Budapest', 'Bucharest', 'Istanbul', 
'Belgrade', 'Barcelona']
```

It looks like our algorithm have a time complexity of $\mathcal{O}(10^{n-10})$ where $n$ is the number of cities visited. We can use this to estimate that it will take ~ $10^{14}$  seconds or ~ $10^9$ days to calculate the optimal answer for all 24 cities. 

For 6 and 10 cities we got the following final distance of the routes:

```console
The optimal distance on 6  cities is: 5018.8100
The optimal distance on 10 cities is: 7486.3100
```

![Exhaustive search performed on 6 cities.](../exhaustive6.png){width=70%}

![Exhaustive search performed on 10 cities.](../exhaustive10.png){width=70%}


## 3. Hill Climbing

We will now write a hill climbing algorithm to solve TSP. 
Instead of the standard algorithm we implement a steepest ascent hill climbing algorithm that looks through all the possible swaps and chooses the optimal one each iteration. (You can run the algorithm without steepest ascent in the notebook). 

We run the algorithm 20 times on both the first 10 cities and all 24 cities, and get the following output:

```console
num_cities = 10
min_dist   = 7486.3100
max_dist   = 8391.0500
average    = 7580.5224
std        = 205.4701
#######################
num_cities = 24
min_dist   = 12362.9200
max_dist   = 16249.1600
average    = 14277.2900
std        = 960.2069
```

When calculating the best distance found after 20 runs, we get the following output:

```console
The best distance found for 10 cities is:  7486.3100
The best distance found for 24 cities is: 13301.4700
```

The results show that the optimal solution is found for 10 cities, since it is the same distance found with exhaustive search in the previous task. 


We also plot the best route acheived during the 20 runs for both 10 and 24 cities, they are shown in figure 3 and figure 4. 


![Hill climbing algorithm performed on 10 cities.](../hill10.png){width=70%}

![Hill climbing algorithm performed on 24 cities.](../hill24.png){width=70%}

\newpage
## 4. Genetic algorithm

Next, we will write a genetic algorithm (GA) to solve TSP. We choose order crossover as our crossover operation since it will preserve an (hopefully) good order, and a standard swap mutation. We use a population size of 10, and train the model with 50 mutations and 100 generations. 

When running the program on 24 cities with a population size of 6, 12 and 18 we get the following output:

```console
pop_size   = 6
min_dist   = 13745.1400
max_dist   = 15622.3400
average    = 14376.7033
std        = 521.2529
The best distance found for 24 cities is: 13745.1400
#####################################
pop_size   = 12
min_dist   = 13468.7000
max_dist   = 15284.3200
average    = 14431.6619
std        = 501.1864
The best distance found for 24 cities is: 13468.7000
#####################################
pop_size   = 18
min_dist   = 13635.9000
max_dist   = 15190.2800
average    = 14286.9986
std        = 364.2537
The best distance found for 24 cities is: 13635.9000
```

which shows that our genetic algorithm performs a little worse than steepest ascent hill climbing, but that it still gives a reasonable answer. 
The average fitness of the best fit individual in each generation is shown for all three population sizes in figure 5 below. 

![Average fitnes of the best fit individual in each generation for 24 cities with varying population sizes.](../genetic_performance.png)


The best plans from the 20 runs for 10 and 24 cities is shown in figures 6 and 7 below. 

![Genetic algorithm performed on 10 cities.](../genetic10.png){width=70%}


![Genetic algorithm performed on 24 cities.](../genetic24.png){width=70%}

\newpage
When running the algorithm for 10 cities and three different population sizes, we get the following results:


```console
pop_size   = 6
min_dist   = 7486.3100
max_dist   = 7486.3100
average    = 7486.3100
std        = 0.0000
The best distance found for 10 cities is: 7486.3100
#####################################
pop_size   = 12
min_dist   = 7486.3100
max_dist   = 7486.3100
average    = 7486.3100
std        = 0.0000
The best distance found for 10 cities is: 7486.3100
#####################################
pop_size   = 18
min_dist   = 7486.3100
max_dist   = 7486.3100
average    = 7486.3100
std        = 0.0000
The best distance found for 10 cities is: 7486.3100
```

which shows that the optimal distance is found (same as exhaustive search) for all three population sizes. 

The genetic algorithm uses 0.2753 s to run with a population size of 6, 0.5129 s with a population size of 12 and 
0.7780 s with a population size of 18. The exhaustive search algorithm used ~1 s to compute the optimal solution, therefore the genetic algorithm is faster. 
With 24 cities it would take way to long to compute using exhaustive search (as discussed in section 2). Therefore we must rely on alternative methods such as genetic algorithms, although they might not find the global minimum. 

My genetic algorithm inspects the route approximately
$n^2$ times, where $n$ is the number of cities. 
In comparison, exhaustive search inspects all possible routes which is of order $n!$. 



## 5. Combination of the genetic algorithm and steepest ascent hill climbing (extra)


If we run our steepest ascent hill climbing algorithm for a small amount of iterations (10) on the childs produced each generation, we get a much more accurate algorithm. 

We run the algorithm with num_mutations set to 20 for 10 generations and a population size of 6 and get the following output:

```console
time used for one run : 0.7226 s.
pop_size   = 6
min_dist   = 12287.0700
max_dist   = 12896.0300
average    = 12504.3014
std        = 200.0327
````

The figure showing the best plan is shown below in figure 8. 


![Hybrid algorithm performed on 24 cities.](../hybrid24.png){width=70%}