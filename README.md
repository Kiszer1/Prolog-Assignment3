# Prolog-Assignment3

Kakuro with BEE
In the game of Kakuro you are required to fill horizontal and vertical blocks with distinct
integers between 1 and 9 which sum to a given clue. (you can read more about the game of
Kakuro on Wikipedia) and here is an example Kakuro board (left) with its solution (right):
We represent Kakuro instances as a list of blocks. Each element of the list is of the form
ClueSum = Block, where ClueSum is an integer and Block is a list of the block’s variables.

14 = [I11, I12, I13, I14], 17 = [I3, I4, I5, I6], 3 = [I7, I8], 4 = [I9, I10], 11 = [I3, I7],
6 = [I1, I2], 8 = [I4, I8, I11], 3 = [I1, I5], 18 = [I2, I6, I9, I13], 3 = [I10, I14]] 
A legal Kakuro solution is a list of terms as described above, which contains distinct
integers between 1 and 9 instead of variables, such that each block’s sum is equal to the
clue

14 = [5, 1, 6, 2], 17 = [9, 2, 1, 5], 3 = [2, 1], 4 = [3, 1], 11 = [9, 2],
6 = [2, 4], 8 = [2, 1, 5], 3 = [2, 1], 18 = [4, 5, 3, 6], 3 = [1, 2]] 


Task 1: kakuroVerify (10 %)
Write a Prolog predicate kakuroVerify(Instance, Solution) with mode
kakuroVerify(+,+), which succeeds if and only if its first argument represents a legal
Kakuro instance, and its second argument represents a legal solution to that instance.


Task 2: kakuroEncode (25 %)
Write a Prolog predicate kakuroEncode(Instance,Map,Constraints). The predicate
has mode kakuroEncode(+,-,-). The predicate takes an instance of Kakuro, and encodes
it to a set of BEE constraints Constraints which is satisfiable if and only if the mapping
of BEE integer variables specified in Map is a solution to the Kakuro instance.
1


Task 3: kakuroDecode (5 %)
Write a Prolog predicate kakuroDecode(Map,Solution) with mode kakuroDecode(+,-),
which decodes the Map of variables generated by kakuroEncode/3 to a Kakuro solution.


Task 4: kakuroSolve (10 %)
Write a Prolog predicate kakuroSolve(Instance,Solution), with mode
kakuroSolve(+,-) that takes a Kakuro instance, solves it using BEE. When solving you
must encode the instance to BEE constraints, decode the solution and verify it. We
recommend you use the BEE framework to implement kakuroSolve
Killer Sudoku with BEE
A Killer Sudoku is a 9 × 9 Sudoku puzzle. In this type of puzzle one must fill the values
of cells according to the following constraints
1. Each row, column and box consist of all-different values (between 1 and 9).
2. Any two cells separated by a Knight’s move (the chess-piece) must be different.
3. Any two cells separated by a King’s move (the chess-piece) must be different.
4. The values of any two cells which appear next to each other in a row, or column
must have absolute difference of at least 2.
For example, the following are a Killer Sudoku puzzle and its solution.
[[_, _, _, _, _, _, _, _, _], [[4, 8, 3, 7, 2, 6, 1, 5, 9],
[_, _, _, _, _, _, _, _, _], [7, 2, 6, 1, 5, 9, 4, 8, 3],
[_, _, _, _, _, _, _, _, _], [1, 5, 9, 4, 8, 3, 7, 2, 6],
[_, _, _, _, 6, _, _, _, _], [8, 3, 7, 2, 6, 1, 5, 9, 4],
[_, _, 1, _, _, _, _, _, _], [2, 6, 1, 5, 9, 4, 8, 3, 7],
[_, _, _, _, _, _, 2, _, _], [5, 9, 4, 8, 3, 7, 2, 6, 1],
[_, _, _, _, _, _, _, _, 8], [3, 7, 2, 6, 1, 5, 9, 4, 8],
[_, _, _, _, _, _, _, _, _], [6, 1, 5, 9, 4, 8, 3, 7, 2],
[_, _, _, _, _, _, _, _, _]] [9, 4, 8, 3, 7, 2, 6, 1, 5]]
A Killer Sudoku puzzle is represented by a list of assignment constraints (hints). The
above puzzle is represented by the term:
Instance = killer([cell(5,3) = 1, cell(6,7) = 2, cell(4,5) = 6, cell(7,9) = 8])
A solution is represented as a list of assignment constraints:
Solution = [cell(1,1) = 4, cell(1,2) = 8, cell(1,3) = 3, ..., cell(9,9) = 5]
Notice that (like a normal Sudoku puzzle) a Killer Sudoku puzzle is legal only in case
it has exactly one solution.
2


Task 5: Killer Sudoku Verifier (10%)
Write a Prolog predicate verify_killer(Instance,Solution) with mode
verify_killer(+,+) which succeeds if and only if its second argument Solution represents a legal Killer Sudoku solution for the instance Instance in its first argument.
Otherwise, verify_killer should fail. The instance is represented as a term
Instance=killer(Hints) where Hints is a list of assignment constraints (hints) and the
solution is represented as a list with 81 assignment constraints, one for each cell of the
9 × 9 board. This predicate does not verify the uniqueness of the Solution, only that it
assigns valid values to the Instance cells. This predicate must also be deterministic. In
this task you should assume that the given Instance is a legal Killer Sudoku instance.
For example,
?- Instance = killer([cell(5,3) = 1, cell(6,7) = 2, cell(4,5) = 6, cell(7,9) = 8]),
Solution = [cell(1,1) = 4,cell(1,2) = 8,...,cell(9,9) = 5], % full solution above
verify_killer(Instance, Solution).
true.
?- Instance = killer([cell(5,3) = 1, cell(6,7) = 2, cell(4,5) = 6, cell(7,9) = 8]),
Solution = [cell(1,1) = 8, ..., cell(2,3) = 8, ...],
verify_killer(Instance, Solution).
false.
?- Instance = killer([cell(5,3) = 1, cell(6,7) = 2, cell(4,5) = 6, cell(7,9) = 8]),
Solution = [cell(1,1) = 8, ..., cell(2,1) = 9, ...],
verify_killer(Instance, Solution).
false.


Task 6: Killer Sudoku Encoder (20%)
Write a Prolog predicate encode_killer(Instance, Map, Constraints) with mode
encode_killer(+, -, -) which given a Killer Sudoku instance which is represented by a
list of hints (assignment constraints) Instance = killer(Hints) unifies Map with a representation of the instance as a list of 81 terms of the form cell(I,J) = Value, where
I and J are Prolog integers representing the index of the cell, and Value represents a
BEE integer between 1 and 9 encoded in Constraints. A call to the predicate instantiates Constraints with a set of BEE constraints such that the satisfying assignments for
Constraints correspond to solutions of the Killer Sudoku instance Instance and bind
the variables in Map to represent the corresponding integer values. In this task you should
assume that the given Instance is a legal Killer Sudoku instance.


Task 7: Killer Sudoku Decoder (10%)
Consider the following predicate to solve Killer Sudoku puzzles using a sat solver.
solve_killer(Instance, Solution) :-
runExpr(Instance, Map, encode_killer, decode_killer, verify_killer).
3
The call to runExpr/5 should succeed and bind the variables in Map to a representation
of integer values (using the representation of BEE).
Write the predicate decode_killer(Map,Solution) with mode decode_killer(+, -)
which creates from the given Map a corresponding solution in the format which is a list of
(81) assignment constraints.


Task 8: Legal Killer Sudoku Instance (10%)
Write a Prolog predicate all_killer(Instance, Solutions) with mode all_killer(+, -)
which given an instance of a Killer Sudoku puzzle unifies Solutions with a list of all possible solutions to Instance. This predicate must be deterministic. If Instance is a legal
Killer Sudoku instance then Solutions will contain exactly one solution.
