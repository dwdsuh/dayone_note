---
title: "[CS] Searhing Strategy"
date: 2019-07-23T10:34:23+09:00
draft: false
---
### Greedy Search

- an algorithmic paradigm that follows the problem solving heuristic of making the locally optimal choice at each stage. 
- traveling salesman problem: At each step of the journey, visit the nearest unvisited city. 
- Does Not ensure the global optimum, but returns locally optimal solutions.



### Beam Search

- beam width: the number of candidates that we are going to examine at each step.
  - If the beam width ==1, it is greedy search. 
- At each step, the algorithm examines all candidates and choose top-N candidate to go further




