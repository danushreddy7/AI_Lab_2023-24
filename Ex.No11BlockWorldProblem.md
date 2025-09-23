# Ex.No: 11  Planning –  Block World Problem 
### DATE:23/09/2025                                                                          
### REGISTER NUMBER : 212223040029
### AIM: 
To find the sequence of plan for Block word problem using PDDL  
###  Algorithm:
Step 1 :  Start the program <br>
Step 2 : Create a domain for Block world Problem <br>
Step 3 :  Create a domain by specifying predicates clear, on table, on, arm-empty, holding. <br>
Step 4 : Specify the actions pickup, putdown, stack and un-stack in Block world problem <br>
Step 5 :  In pickup action, Robot arm pick the block on table. Precondition is Block is on table and no other block on specified block and arm-hand empty.<br>
Step 6:  In putdown action, Robot arm place the block on table. Precondition is robot-arm holding the block.<br>
Step 7 : In un-stack action, Robot arm pick the block on some block. Precondition is Block is on another block and no other block on specified block and arm-hand empty.<br>
Step 8 : In stack action, Robot arm place the block on under block. Precondition is Block holded by robot arm and no other block on under block.<br>
Step 9 : Define a problem for block world problem.<br> 
Step 10 : Obtain the plan for given problem.<br> 
     
### Program:
# domain.pddl:
```
(define (domain blocksworld)
(:requirements :strips :equality)
(:predicates (clear ?x)
(on-table ?x)
(arm-empty)
(holding ?x)
(on ?x ?y))
(:action pickup
:parameters (?ob)
:precondition (and (clear ?ob) (on-table ?ob) (arm-empty))
:effect (and (holding ?ob) (not (clear ?ob)) (not (on-table ?ob))
(not (arm-empty))))
(:action putdown
:parameters (?ob)
:precondition (and (holding ?ob))
:effect (and (clear ?ob) (arm-empty) (on-table ?ob)
(not (holding ?ob))))
(:action stack
:parameters (?ob ?underob)
:precondition (and (clear ?underob) (holding ?ob))
:effect (and (arm-empty) (clear ?ob) (on ?ob ?underob)
(not (clear ?underob)) (not (holding ?ob))))
(:action unstack
:parameters (?ob ?underob)
:precondition (and (on ?ob ?underob) (clear ?ob) (arm-empty))
:effect (and (holding ?ob) (clear ?underob)
(not (on ?ob ?underob)) (not (clear ?ob)) (not (arm-empty)))))
```
### Input :
# problem-1:
```
(define (problem pb1)
(:domain blocksworld)
(:objects a b)
(:init (on-table a) (on-table b) (clear a) (clear b) (arm-empty))
(:goal (and (on a b))))
```
# problem-2:
```
(define(problem pb3)
(:domain blocksworld)
(:objects a b c)
(:init (on-table a) (on-table b) (on-table c)
(clear a) (clear b) (clear c) (arm-empty))
(:goal (and (on a b) (on b c))))
```
### Output/Plan:
# problem-1:

<img width="544" height="801" alt="Screenshot 2025-09-23 092437" src="https://github.com/user-attachments/assets/280ae772-f29c-4d12-aa4f-c1d38c78639a" />

<img width="573" height="803" alt="Screenshot 2025-09-23 092625" src="https://github.com/user-attachments/assets/0536bea0-ea3f-4436-b7fb-79c202b31cfd" />

# problem-2:

<img width="513" height="798" alt="Screenshot 2025-09-23 092907" src="https://github.com/user-attachments/assets/bb4c5814-8732-4ce5-8f64-acef65678294" />

<img width="492" height="805" alt="Screenshot 2025-09-23 092948" src="https://github.com/user-attachments/assets/effb9611-5dba-496c-b16e-2f03e1af6869" />

<img width="502" height="808" alt="Screenshot 2025-09-23 093044" src="https://github.com/user-attachments/assets/819f868c-ad78-494b-936b-7f11fb51acb8" />

<img width="499" height="801" alt="Screenshot 2025-09-23 093142" src="https://github.com/user-attachments/assets/0459415a-06a5-4d6f-9434-86d4a9837b12" />

### Result:
Thus the plan was found for the initial and goal state of block world problem.
