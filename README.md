This journal explains the Newton-Raphson method for numerically solving for zeros of a function. It is a recursive method which takes the output from a previous iteration as an input until ideally it converges on a solution. More on this method can be found here: [Newton's Method](https://en.wikipedia.org/wiki/Newton%27s_method) <br>
Here we'll learn how to implement it in Python.

#### Let's find where y = x^3 - 100 crosses the y-axis (y=0). This is a trivial problem, but this method can be quite powerful when extended to multiple dimensions. Please see my other notebook for an example on how to apply this to 3D problems.


```python
# Import libraries
import matplotlib.pyplot as plt
import numpy as np
```

Let's define some basic functions needed. We'll need the function itself, it's derivative, and the equation of the tangent line given a point on the function.


```python
def func(x):               # y = x^3 - 100
    return x ** 3 - 100
def slope(xn):             # slope for tangent of y, dy/dx = 3*x^2, evaluated at some x
    return 3*(xn ** 2)
def tangent(x, xn, yn):    # tangent line
    return slope(xn)*(x - xn) + yn
```

As with many recursive methods, we need to provide an initial guess. We'll also set up our space in x.


```python
# Creating vectors x and y
x = np.linspace(-12, 12, 100)

# Initial Guess
x1 = -6
```

Now let's plot our function, along with the tangent line at our initial guess (x=-6). To get the next guess, we'll grab the x-value when y=0 of our tangent line.


```python
fig = plt.figure(figsize = (12, 7))
# Create the plot
plt.plot(x, func(x), alpha = 0.6, label ='y = x^3 - 100',
         color ='green',
         linewidth = 2)
xtan = np.linspace(x1-5, x1+5, 100)
plt.plot(xtan, tangent(xtan,x1,func(x1)), alpha = 0.8, label ='tangent at x=-6',
         color ='red',
         linewidth = 3)
plt.vlines(x=x1, ymin=-2000, ymax=func(x1), ls=':', lw=3,label ='initial guess')
plt.vlines(x=(-func(x1) + slope(x1)*x1) / (slope(x1)), ymin=-2000, ymax=tangent(x1, x1, 0), ls=':', lw=3,
           label ='new guess',color='red')

# Add a title
plt.title("Newton-Raphson First Iteration")
 
# Add X and y Label
plt.xlabel('x axis')
plt.ylabel('y axis')
 
# Add a grid
plt.grid(alpha =.6, linestyle ='--')
 
# Add a Legend
plt.legend()

# Show the plot
plt.show()
```


    
![png](output_8_0.png)
    


As you can see, the tangent line intercepts the x-axis at x~-3.07


```python
# y2 - y1 = m*(x2 - x1)  point slope
# x2 = (-y1 + m*x1)/m    algebra
x_new_guess = (-func(x1) + slope(x1)*x1) / (slope(x1))
x2 = x_new_guess
x2
```




    -3.074074074074074



Now let's use our new guess and plot the tangent line at x=-3.07, and again obtain the x-intercept


```python
fig = plt.figure(figsize = (12, 7))
# Create the plot
plt.plot(x, func(x), alpha = 0.6, label ='y = x^3 - 100',
         color ='green',
         linewidth = 2)
xtan = np.linspace(x2-5, x2+5, 100)
plt.plot(xtan, tangent(xtan,x2,func(x2)), alpha = 0.8, label ='tangent at x=-6',
         color ='red',
         linewidth = 3)
plt.vlines(x=x2, ymin=-2000, ymax=func(x2), ls=':', lw=3,label ='initial guess')
plt.vlines(x=(-func(x2) + slope(x2)*x2) / (slope(x2)), ymin=-2000, ymax=tangent(x2, x2, 0), ls=':', lw=3,
           label ='new guess',color='red')

# Add a title
plt.title("Newton-Raphson Second Iteration")
 
# Add X and y Label
plt.xlabel('x axis')
plt.ylabel('y axis')
 
# Add a grid
plt.grid(alpha =.6, linestyle ='--')
 
# Add a Legend
plt.legend()

# Show the plot
plt.show()
```


    
![png](output_12_0.png)
    



```python
# We can do this again for the next guess
x_new_guess = (-func(x2) + slope(x2)*x2) / (slope(x2))
x3 = x_new_guess
x3
```




    1.4779797458463932



As we continue this process, you'll notice that this converges on a particular point. We can automate this process in a loop.

#### Now let's automate the first 10 guesses


```python
x_guess = x1
for i in range(10):
    x_new_guess = (-func(x_guess) + slope(x_guess)*x_guess) / (slope(x_guess))
    x_guess = x_new_guess
    print(x_guess)
```

    -3.074074074074074
    1.4779797458463932
    16.244871713730852
    10.956226929956188
    7.58183902754217
    5.634427997014628
    4.806260621343697
    4.647166372674656
    4.64159552510902
    4.641588833622426
    

Let's check the final guess (at x~4.64)


```python
fig = plt.figure(figsize = (12, 7))
# Create the plot
plt.plot(x, func(x), alpha = 0.6, label ='y = x^3 - 100',
         color ='green',
         linewidth = 2)
plt.plot(x_guess,func(x_guess),'ro')
# Add a title
plt.title("Final Guess")
 
# Add X and y Label
plt.xlabel('x axis')
plt.ylabel('y axis')
 
# Add a grid
plt.grid(alpha =.6, linestyle ='--')
print(f"Final guess after 10 iterations: {round(x_guess,4)}, yielding {round(func(x_guess),3)}\nThat's the correct answer!")
```

    Final guess after 10 iterations: 4.6416, yielding 0.0
    That's the correct answer!
    


    
![png](output_18_1.png)
    


That's all there is to it! We've successfully used a numerical method to find where y = x^3 - 100 crosses the x-axis. The solution for this example problem was trivial, but one can see that with only a few iterations the correct solution can be obtained without a calculator.
