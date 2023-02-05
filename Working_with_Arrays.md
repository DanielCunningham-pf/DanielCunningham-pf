This code takes the arrays a,b,c which are defined at the beginning of the question, and loops over them to print out their contents in columns and add a row containing a number from each column. However, the final output should not contain any duplicates where the order of the numbers across the arrays does not matter, i.e [1,4,8] = [8,4,1]


```python
import numpy as np
from sympy import *
```


```python
a = np.array([1,1,1,2,8,6,1])   #Create arrays for a,b,c as set up intially in the question.
b = np.array([2,4,6,1,4,18,61])
c = np.array([2,8,18,2,1,1,8])

sum_ = []                       #Create empty arrays for 'sum' and 'delete' to append to.
delete = []

for i in range(0,7):            #Ranges from (0-6) as loops over to the term before the final term
    sums = a[i] + b[i] + c[i]   #Calculates the sum of the 1st - 7th terms in the a,b,c arrays Same numbers 
                                #but in different would have the same total when added.
    
    if sums in sum_:            #Checks to see if sum is alraedy in array
        delete.append(i)        #If sum is in array, it adds the position of the numbers in a,b,c 
                                #to be deleted into the array 'delete'.
   
    else:                      
        sum_.append(sums)       #Adds Sum of combination of numbers to array 'sum_'.
    
n1 = np.delete(a,delete)        #Deletes repeating combinations of numbers from arrays 'a,b,c'. 
n2 = np.delete(b,delete)
n3 = np.delete(c,delete)

con = np.stack((n1, n2, n3), axis=1)    #Combines all arrays with deleted repeating combinations to one array.
Headings = np.array(['a', 'b', 'c'])   #Creates an array for the column names.
final = np.vstack((Headings,con))       #Adds Column headings to top of array of combination of arrays.

F = Matrix([final])                     #Creates a Matrix so that output is presented clearly.
J = Matrix.transpose(F)                 #Transposes the matrix (swaps rows and columns)
J                                       #Prints Final Output
```




$\displaystyle \left[\begin{matrix}\left[\begin{matrix}a & b & c\end{matrix}\right]\\\left[\begin{matrix}1 & 2 & 2\end{matrix}\right]\\\left[\begin{matrix}1 & 4 & 8\end{matrix}\right]\\\left[\begin{matrix}1 & 6 & 18\end{matrix}\right]\\\left[\begin{matrix}1 & 61 & 8\end{matrix}\right]\end{matrix}\right]$




```python

```
