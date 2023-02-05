This Code is written for scientific computing when I was working with a Peltier Device and needed to interact with the box in real time using python. There were 4 input channels and one output channel that I was using to communicate with the Peltier Device. Data is appended to arrays so that graphs can be plotted for temperature difference, voltage and current against time which are then used to calculate thermal conductivity, k, of the Peltier device as well as the Seeback Coefficient. 


```python
import numpy as np 
import matplotlib.pyplot as plt 
import matplotlib.widgets as widgets 
import time 
import sys 
sys.path.insert(1,"C:\\python") 
import y2daq 

def switchoffCallback(event): 
    global switchon 
    global Voff 
    switchon = False; 
    print('switched off') 

     
def SliderCalback(val): 
    print('The current value is: ', val) 
    plt.draw() 

fig=plt.figure(figsize=(7,5))    

# Button to stop acquisition 
offax=plt.axes([0.8,0.75,0.1,0.1]) 
offHandle=widgets.Button(offax,'off') 
offHandle.on_clicked(switchoffCallback)  

#Slider Handle 
sax = plt.axes([0.1, 0.05, 0.6, 0.03]) 
sliderHandle = widgets.Slider(sax, 'Delta_T', -0.04, 0.04, valinit=0) 
sliderHandle.on_changed(SliderCalback) 

a = y2daq.analog() # create analog data acquisition object 
a.addInput(0) 
a.addInput(1) 
a.addInput(2) 
a.addInput(3)  
a.addOutput(0)     # Set up analogue output channel 
a.Nscans = 100     # no. of points per acquisition 
a.Rate =5000       # no. of points per second 

 
# initialize arrays 
y=np.array([]) 
sy=np.array([]) 
x=np.array([]) 
y1 = np.array([]) 

 
ax1=plt.axes([0.13,0.17,0.35,0.30])  
ax1.set_title('Peltier Voltage') 
ax1.set_ylabel('Voltage (V)') 
ax1.set_xlabel('time(s)') 
ax2=plt.axes([0.13,0.60,0.35,0.30])  
ax2.set_title('temperature difference') 
ax2.set_ylabel('Difference in temp (Kelvin)') 
ax2.set_xlabel('Time (s)') 
ax3 = plt.axes([0.63,0.17,0.35,0.30]) 
ax3.set_title('Peltier Current') 
ax3.set_xlabel('time(s)') 
ax3.set_ylabel('Current (A)') 

x0 = time.time() 
switchon = True 

# acquisition loop 

while switchon == True: 
    x1=time.time() 
    data,timestamps = a.read() # acquire data from generator 
    newx = x1-x0+np.mean(timestamps) # time elapsed in seconds 
    new_pv = np.mean(data[0])   # measured voltage at time x 
    new_spv = np.std(data[0])/np.sqrt(a.Nscans) # standard error of mean 
    new_tu = np.mean(data[2]) 
    new_stu = np.std(data[2])/np.sqrt(a.Nscans) 
    new_pc = np.mean(data[1]) 
    new_spc = np.std(data[1])/np.sqrt(a.Nscans) 
    new_tl = np.mean(data[3]) 
    new_stl = np.std(data[3])/np.sqrt(a.Nscans) 
    new_mean_tempdiff = new_tu-new_tl 
    new_setd = np.sqrt((new_stu)**2 + (new_stl)**2) 
    ax1.errorbar(newx,new_pv,yerr=new_spv,fmt='b.') 
    ax2.errorbar(newx, new_mean_tempdiff, yerr=new_setd, fmt='y.') 
    ax3.errorbar(newx,new_pc,yerr=new_spc,fmt='g.') 
    if new_mean_tempdiff > sliderHandle.val:     #if/else positive feedback loop 
        a.write(3) 
    else: 
        a.write(-3) 

    plt.pause(0.01) 
    x = np.append(x,newx)   
    y = np.append(y,new_pv)   
    sy = np.append(sy,new_spv) 
    y1 = np.append(y1, new_mean_tempdiff) 
    sy = np.append(sy, new_setd) 
```


    ---------------------------------------------------------------------------

    ModuleNotFoundError                       Traceback (most recent call last)

    <ipython-input-1-2840dc45df34> in <module>
         11 sys.path.insert(1,"C:\\python")
         12 
    ---> 13 import y2daq
         14 
         15 
    

    ModuleNotFoundError: No module named 'y2daq'



```python

```
