##############################################################################################################
######################################          SUDOKU SOLVER          #######################################
##############################################################################################################


#########################################          Summary          ##########################################

Sudoku is a mathematical game, in which the player needs to fill a square matrix 9 x 9 (81 cells) divided in 
blocks 3 x 3 (9 cells) with values from 1 to 9, and the rule is: these values can only appear once in each row, 
column and block, taking the fact that some of those cells are already pre-filled.

Although there are different variants of this game / puzzle (using colors, alphabetical characters, figures, etc), 
the present work approaches the study of the classic description (previously mentioned) throughout the implementation 
of backtracking search for CSP in Python. We can look at different ways to model this algorithm like


########################################          Background          #########################################

The backtracking search is an algorithm used in computational problems with the premise of implementing a 
depth-first search, it consists of creating a set of solutions by picking values for a variable {Xn} at a time 
and backtracks when a variable has no more possible values and the action cannot be completed for a valid solution 
based on the constrains, and here is where recursive method is used to iterate until find a solution. If there is 
no solution that meets with all constrains and/or criteria, then the algorithm´s output will return a failure.

It should be noticed that, there different types of problems in backtracking: [1]
 
*Decision Problem – In this, we search for a feasible solution.
*Optimization Problem – In this, we search for the best solution.
*Enumeration Problem – In this, we find all feasible solutions.


###################################          Why backtracking search?         ###################################

Since the sudoku is an iterative process where we try filling cells one at time and then verify in every step if 
this meets with the rule, otherwise we need to change any other previous step to continue, it is reasonable to 
choose an algorithm that allows us to follow this path and backtracking search is used under this concept 
(choosing an option, then choose one of a new set of options and we can repeat this process “n” times, over and 
over again until we reach the goal or final state), this is better than generating all possible combinations of 
digits and then trying every single option.

Since the backtracking algorithm uses the approach of deep-first search is easy to see that this type of algorithm 
can be implemented to find all feasible solutions in our problem if they exist, but a weak point of using this kind 
of method is that: backtracking is not considered an optimized technique to solve the problem as breadth-first search 
could be, so this is a brute force approach with constrains.


###################################          Backtracking performance          ###################################

In regards of time and space complexity we can describe it as follow: 

In first instance we can consider a single empty cell, so the possible values that this can get is one of the elements 
in our domain (the set of positive integer numbers from 1 to 9 D = {1, 2, 3, …, 9}), then we have this “n” possibilities 
and the worst case would be that our solution was the last element in the set (exploring all set), based on that we need 
to repeat the process by considering every empty cell. In addition, we need to consider the fact the algorithm uses 
depth-first search, where each level in the graph is the choices for a single cell, so the depth of the graph is the number 
of cells that need to be filled, with branching factor of “n” and a depth of “m”, so the time complexity turns into: O(n^m). 
On the other hand, as result of we must fill the square matrix 9 x 9 and store it, the space complexity is O(n*n).


###################################          How does the code work?          ###################################

In first instance, the very first decision is to ensure that our grid does NOT contain any value (different from zero) 
more than once, otherwise it would be a waste of time by trying to solve a puzzle with no solution, for this reason 
“revise” function is used;  if that was the case the square matrix (9 x 9) full of -1 is returned.

If the grid meets the first acceptance criteria to continue with the operations, we have 2 different approaches 
to solve the sudoku, both use backtracking search as algorithm, the first one, which is the main uses a constrain 
propagation method as R. Stuart and P. Norvig [2], where every single empty cell (cells with 0 value) is filled with
 the full domain {1, 2, 3, 4, 5, 6, 7, 8, 9} (“replacing_zeros” function), then the code collects all fixed values 
(initial numbers different from zero in the cells) per row, column and block, this way the numbers are removed from 
the corresponding domain of the cells based on the main rule of the game, assigning a different number each time from 
“backtracking” function, repeating the process and reducing the candidates until have only one, which will be set in 
the cell in turn. 

The “backtracking” function achieved the goal if the sudoku is filled, returning the cells of the grid with a number 
that meets the rule, if this never happen the function returns the matrix full of -1 as result. On the other hand, 
a final assessment is required as sanity check, this is using again the function: “revise” to ensure everything is ok. 
Now if this last test is failed the second backtracking search is called to double-check and search for any other solution, 
this time by using a different approach, which is simpler than previous one and it just assigns a number from the 
domain in every empty cell from left to right and from top to bottom, this approach covers other feasible solutions, 
however the process time for hard sudokus is increased. Once that, if the “backtracking_backup” gets a solution; 
this is returned, otherwise the matrix full of -1 is printed.

One thing should be notice at this point, there are different methods / approaches to use for the backtracking search, 
I explore 4 variants: 

1.	Assigning a number to the first cell from left to right and from top to bottom.
2.	Searching and filling the row with a fewer 0s and so on (minimum remaining-values (MRV) for rows).
3.	Making a comparison between rows and columns with a fewer 0s and then fill that row or column 
	(minimum remaining-values (MRV) for columns and rows).
4.	Constrain propagation for rows, columns and blocks + Making a comparison between rows and columns 
	with a fewer 0s and then fill that row or column (minimum remaining-values (MRV) for columns and rows).

Each approach showed in some cases very close process time for very easy, easy and medium sudokus, however only the 
method 3 and 4 present an improvement for most of hard sudokus. In the table below, we can see the process time for 
each method (this time was measured a couple of times, it does NOT represent a mean). In spite of the method 1 and 2 
show better times, I decided to implement the method 4 which is a combination 
of two  approaches as result of this could solve the hard sudokus in less time.

Very Easy (Process Time [sec])

Sudoku	Mehod 1	Mehod 2	Mehod 3	Mehod 4
0	0.03125	0.03125	0.03125	0.0625
1	0.03125	0.0625	0.03125	0.046875
2	0.03125	0.03125	0.01562	0.03125
3	0.04687	0.03125	0.03125	0.046875
4	0.01562	0.03125	0.03125	0.078125
5	0.03125	0.0625	0.04687	0.046875
6	0.0625	0.03125	0.03125	0.03125
7	0.03125	0.01562	0.03125	0.03125
8	0.01562	0.03125	0.03125	0.046875
9	0.03125	0.03125	0.03125	0.046875
10	0.01562	0.03125	0.04687	0.03125
11	0.03125	0.01562	0.03125	0.03125
12	0.01562	0.01562	0.03125	0.046875
13	0.01562	0.01562	0.03125	0.03125
14	0.01562	0.01562	0.01562	0.046875


Easy (Process Time [sec])

Sudoku	Mehod 1	Mehod 2	Mehod 3	Mehod 4
0	0	0	0.01562	0.03125
1	0.01562	0.03125	0	0.03125
2	0.01562	0	0.01562	0.03125
3	0	0	0	0.03125
4	0.03125	0.01562	0.01562	0.015625
5	0.04687	0.04687	0.01562	0.0625
6	0.03125	0.03125	0.01562	0.0625
7	0.03125	0.04687	0.03125	0.046875
8	0.01562	0.03125	0.01562	0.03125
9	0.01562	0.03125	0.04687	0.0625
10	0.03125	0.03125	0.0625	0.03125
11	0.0625	0.01562	0.01562	0.046875
12	0.03125	0.03125	0.01562	0.03125
13	0.03125	0.01562	0.03125	0.046875
14	0.03125	0.04687	0.01562	0.09375


Medium (Process Time [sec])

Sudoku	Mehod 1	Mehod 2	Mehod 3	Mehod 4
0	0.01562	0.03125	0.03125	0.0625
1	0.01562	0.01562	0.04687	0.03125
2	0.03125	0.04687	0.01562	0.015625
3	0.0625	0.01562	0.03125 0.015625
4	0.03125	0.03125	0.01562 0.015625
5	0.04687	0.07812	0.03125	0.0625
6	0.04687	0.04687	0.03125	0.03125
7	0.0625	0.04687	0.04687	0.046875
8	0.04687	0.04687	0.03125	0.0625
9	0.03125	0.03125	0.04687	0.046875
10	0.04687	0.03125	0.0625	0.0625
11	0.04687	0.04687	0.07812	0.046875
12	0.03125	0.04687	0.04687	0.03125
13	0.04687	0.0625	0.04687	0.046875
14	0.0625	0.03125	0.04687	0.0625


Hard (Process Time [sec])

Sudoku	Mehod 1	Mehod 2	Mehod 3	Mehod 4
0	38.5	14.7031	13.2656	28.453125
1	458.968	0.48437	8.375	33.0625
2	140.437	54.7343	55.2187	61.34375
3	17.2343	343.25	16.9062	29.171875
4	60.8906	6.125	5.98437	8.390625
5	338.953	19.8437	20.2343	20.53125
6	10.7343	301.578	237.046	11.953125
7	4.35937	26.8281	25	0.890625
8	31.8593	83.5781	120.828	11.359375
9	46.9687	T/E	1020.59	1007
10	685	T/E	72.4062	92.453125
11	432.312	T/E	62.3437	71.03125
12	164.375	T/E	T/E	19.9375
13	2.48437	T/E	T/E	13.21875
14	333.093	T/E	T/E	115.48438

T/E = time exceeded


########################################          Conclusion          #########################################

By studying different search algorithms we can notice that all of them have pros and cons and each one could be 
implemented to solve a specific problem, this time a backtracking search was used to solve a classical game (sudoku), 
this algorithm implements the approach of deep-first search to find feasible solutions, what result in a brute 
force approach with constrains with high process time for some applications, nevertheless backtracking is a valid 
algorithm for solving a variety of problems, even though time complexity.

As we can see: different approaches result in different process time, for easy and medium sudokus a simple approach 
is enough to get a solution, however hard or extreme sudokus require other implementations to decrease in efficient 
way the process time, and as we expect: the algorithm is the one of the main offender to do that.

One thing that can be implemented is: to run 2 or more process / approaches in parallel and if one of them returns 
a feasible solution in less time than the others, the remaining process can stop. 


########################################          References          #########################################


[1]. Chaturvedi A. (2021, February 9). Backtracking | Introduction. Accessed September 18th, 2021 <https://www.geeksforgeeks.org/backtracking-introduction/>

[2]. Russell, Stuart J., and Peter Norvig. Artificial Intelligence : A Modern Approach. 
Third Edition / Contributing Writers, Ernest Davis [and Seven Others].; Global ed. 2016. Prentice Hall Ser. in Artificial Intelligence. Web. 

[3]. Subham D. (2020, November 26).  Backtracking Algorithms. Accessed September 19th, 2021 <https://www.baeldung.com/cs/backtracking-algorithms>



