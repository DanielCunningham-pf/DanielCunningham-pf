This Code calculates the expectation value for momentum using the integrand function, complex conjugate and derivative functions in python for the n=100 eigenstate of the Hamiltonian. 


```python
import numpy as np
import scipy.integrate as integr
from scipy.integrate import quad

N = 10000
n1 = 1                                                    #Defining constants 
n100 = 100
L = 10e-9
x = np.linspace(0,L,N)
hbar = 1.05e-34                                           #Equal to h/2π
operator_constants = -hbar*1j                             #Momentum operator is -iℏ*d/dx so -iℏ are constants in integral                           

u_100 = np.sqrt(2/L) * np.sin(n100*np.pi*x/L)                   
derivative_u100 = np.gradient(u_100)

#Integral is complex conjugate of u_n multiplied by derivative of u_n, due to momentum operator, multiplied by the constants. 
integrand_100 = np.conj(u_100)*derivative_u100*operator_constants

#Find integral with respect to x for u_100
I100 = integr.simps(integrand_100, x)

print('The expectation value of momentum for u_100 is {:.10f}'.format(I100))

#Expectation value for momentum for u_100 is 0, which matches with analytical solutions. 
```

    The expectation value of momentum for u_100 is 0.0000000000-0.0000000000j
    


```python

```
