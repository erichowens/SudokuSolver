Simulated-Annealing Sudoku Solver
=================================

  By Erich Owens 

  August 3rd, 2012

Overview
--------
  Script takes an incomplete sudoku puzzle and uses simulated 
  annealing to find a solution. 
  
  (This code is hardly optimized! Others, like Peter Norvig,
  have written some great solvers. This quickly-written script
  was inspired by John Myles White using simulated-annealing to solve
  Sudoku in the Julia language: https://github.com/johnmyleswhite/sudoku.jl)
  
  Appreciate any comments or feedback. Can be found on Twitter at
  @erich_owens or erich.owens@gmail.com. 

Gritty, Mathy Details
---------------------
  Randomly fills out the incomplete sub-squares to be consistent 
  sub-solutions (have all numbers from 1-9)
    
  Scores a given puzzle by giving a -1 for every unique element 
  in each row or each column. Best solution has a score of -162. 
  (This is a stopping rule.)
    
  Candidate for new puzzle is created by randomly selecting a 
  sub-square, then randomly flipping  two of its entries, evaluating
  the new score. The delta_S is the difference between the scores.
    
  Let T be the global temperature of our system (here initialized 
  as .5), with a geometric schedule for decreasing (perhaps by 
  T <- .99999 T).
    
  If U is drawn uniformly from [0,1], and exp((delta_S/T)) > U,
  then we accept the candidate solution as our new state.

Usage
-----
  Running with no arguments will run a default sudoku puzzle (the
  one found on the [Wikipedia page](http://en.wikipedia.org/wiki/Sudoku)).
  
    Erichs-MacBook-Air:SudokuSolver erichowens$ python sudoku.py 
    Original Puzzle:
    =========================
    | 5 3 ' | ' 7 ' | ' ' ' |
    | 6 ' ' | 1 9 5 | ' ' ' |
    | ' 9 8 | ' ' ' | ' 6 ' |
    =========================
    | 8 ' ' | ' 6 ' | ' ' 3 |
    | 4 ' ' | 8 ' 3 | ' ' 1 |
    | 7 ' ' | ' 2 ' | ' ' 6 |
    =========================
    | ' 6 ' | ' ' ' | 2 8 ' |
    | ' ' ' | 4 1 9 | ' ' 5 |
    | ' ' ' | ' 8 ' | ' 7 9 |
    =========================
    
    Iteration 0,    	T = 0.50000, 	best_score = -122, 	current_score = -122
    Iteration 1000,    	T = 0.49502, 	best_score = -158, 	current_score = -158
    Iteration 2000,    	T = 0.49010, 	best_score = -158, 	current_score = -151
    Iteration 3000,    	T = 0.48522, 	best_score = -158, 	current_score = -151
    Iteration 4000,    	T = 0.48039, 	best_score = -158, 	current_score = -152
    Iteration 5000,    	T = 0.47561, 	best_score = -158, 	current_score = -151
    Iteration 6000,    	T = 0.47088, 	best_score = -158, 	current_score = -155
    Iteration 7000,    	T = 0.46620, 	best_score = -158, 	current_score = -152
    Iteration 8000,    	T = 0.46156, 	best_score = -160, 	current_score = -156
    Iteration 9000,    	T = 0.45697, 	best_score = -160, 	current_score = -157
    Iteration 10000,    	T = 0.45242, 	best_score = -160, 	current_score = -158
    
    SOLVED THE PUZZLE.
    
    Final Puzzle:
    =========================
    | 5 3 4 | 6 7 8 | 9 1 2 |
    | 6 7 2 | 1 9 5 | 3 4 8 |
    | 1 9 8 | 3 4 2 | 5 6 7 |
    =========================
    | 8 5 9 | 7 6 1 | 4 2 3 |
    | 4 2 6 | 8 5 3 | 7 9 1 |
    | 7 1 3 | 9 2 4 | 8 5 6 |
    =========================
    | 9 6 1 | 5 3 7 | 2 8 4 |
    | 2 8 7 | 4 1 9 | 6 3 5 |
    | 3 4 5 | 2 8 6 | 1 7 9 |
    =========================

  Alternatively, one can pass in a puzzle as a single argument on the command line,
  writing the contents of the puzzle in a concatenated sequence by rows, putting
  zeros in for the empty entries.
  
    Erichs-MacBook-Air:SudokuSolver erichowens$ python sudoku.py 207005010003020000654810920070300000802070401000001060045038192000050600010700805
    Original Puzzle:
    =========================
    | 2 ' 7 | ' ' 5 | ' 1 ' |
    | ' ' 3 | ' 2 ' | ' ' ' |
    | 6 5 4 | 8 1 ' | 9 2 ' |
    =========================
    | ' 7 ' | 3 ' ' | ' ' ' |
    | 8 ' 2 | ' 7 ' | 4 ' 1 |
    | ' ' ' | ' ' 1 | ' 6 ' |
    =========================
    | ' 4 5 | ' 3 8 | 1 9 2 |
    | ' ' ' | ' 5 ' | 6 ' ' |
    | ' 1 ' | 7 ' ' | 8 ' 5 |
    =========================
    
    Iteration 0,    	T = 0.50000, 	best_score = -124, 	current_score = -124
    Iteration 1000,    	T = 0.49502, 	best_score = -155, 	current_score = -154
    Iteration 2000,    	T = 0.49010, 	best_score = -156, 	current_score = -155
    Iteration 3000,    	T = 0.48522, 	best_score = -158, 	current_score = -153
    Iteration 4000,    	T = 0.48039, 	best_score = -160, 	current_score = -158
    
    SOLVED THE PUZZLE.
    
    Final Puzzle:
    =========================
    | 2 8 7 | 9 6 5 | 3 1 4 |
    | 1 9 3 | 4 2 7 | 5 8 6 |
    | 6 5 4 | 8 1 3 | 9 2 7 |
    =========================
    | 4 7 1 | 3 8 6 | 2 5 9 |
    | 8 6 2 | 5 7 9 | 4 3 1 |
    | 5 3 9 | 2 4 1 | 7 6 8 |
    =========================
    | 7 4 5 | 6 3 8 | 1 9 2 |
    | 9 2 8 | 1 5 4 | 6 7 3 |
    | 3 1 6 | 7 9 2 | 8 4 5 |
    =========================



  