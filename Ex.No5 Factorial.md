# Ex.No: 5   Logic Programming – Factorial of number   
### DATE:09-09-2025                                                                            
### REGISTER NUMBER :212223040029 
### AIM: 
To  write  a logic program for finding the factorial of given number using SWI-PROLOG. 
### Algorithm:
1. STEP 1: Start the program
2. STEP 2:  Write a rules for finding factorial of given program in SWI-PROLOG.
3.   a)	factorial of 0 is 1 => written as factorial(0,1).
4.   b)	factorial of number greater than 0 obtained by recursively calling the factorial    function.
5. STEP 3: Run the program  to find answer of  query.
6. STEP 4: Stop the program.

### Program:
```
fact(N,RES):-  
           N > 0, 
           N1 is N-1,
           fact(N1,RES1),
           RES is N*RES1.
fact(0,1).
fact(1,1).
```
### Output:
<img width="948" height="360" alt="image" src="https://github.com/user-attachments/assets/a1177ddf-bde5-4ee2-8a82-1fdf154e8dba" />

### Result:
Thus the factorial of given number was found by logic programming. 
