# Custom Functions and Plotting in Python

We will cover two topics this week. Writing custom Python functions and plotting with `matplotlib`.

[Go to Custom Function notes](#python-custom-functions)

[Go to Plotting notes](#plotting-in-python-with-matplotlib)

## Assignment

The assignment for this week can be found in the `Week11Assignment.ipynb` Jupyter notebook, which is in the `Week11Assignments` folder. This assignment will be __due by 5 PM next Thursday (April 16th)__.

## Python Custom Functions

### Jupyter Notebook

Code corresponding to these notes about custom functions in Python can be found in the `CustomFunctions.ipynb` Jupyter notebook in this repository.

### Additional Resources

- [Python Documentation on Defining Functions](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)

### Introduction

The core Python library and optional modules contain a huge number of useful functions. However, as you write more specialized code, you will inevitably need/want to use a function that doesn't yet exist. Thankfully, Python and other programming languages make it easy to write your own custom functions. Any time you find yourself writing the same block of code repeatedly, you should instead put this code in a new function that can be called whenever needed. Writing your own functions makes your code shorter, more readable, more reliable, and easier to debug. The general syntax for defining a custom function looks like this

```
def myNewFunction():
    """ A text description of my function goes here."""
    
    print("Executing a custom function!")
```

The `def` keyword tells Python that we're defining a new function. That is followed by the name of our function (in this case, `myNewFunction`). The function name is then _always_ followed by parentheses - `()`. For simple functions, nothing needs to go inside the parentheses. These are then followed by a colon - `:`.

The next line should be indented and a text string should be provided inside sets of three double quotation marks - `"""`. This text is known as the _docstring_ and provides users, other programmers, or, more likely, you at some later time, with useful information about the function. Executing `help(myNewFunction)` in the Python interpreter will cause the _docstring_ to be printed for the user.

The rest of code relevant to your function should appear on subsequent indented lines after the _docstring_. Simple functions may only have 1 or 2 lines. More complicated functions may have 10s to 100s of lines. However, if the function gets much longer than that, it's probably an indication that you need to break the code up into more functions of a manageable size.

Custom functions are executed just like any core Python function. Simply type the function's name, followed by parentheses.

`myNewFunction()`

### Function Arguments

To make functions more flexible, they can be written to accept arguments. To do this, simply include a new variable name inside the parentheses when defining the function. Note that this variable name _only has meaning within the function's code_. More formally, we say that the scope of the variable is limited to the function.

```
def myFuncWithArg(userProvidedWord):
    """This example function shows how to accept and use arguments."""
    
    print("I am printing the word: %s" % userProvidedWord)

myFuncWithArg("test")
myFuncWithArg("tigers")
```

Note in the example above how we used the new variable name in the function's code, while the value of this variable changed depending on what string the user provided each time they executed the function.

Functions can take more than one argument, as long as they are specified in the function definition.

```
def complexMathThing(numOne,numTwo,numThree):
    """A complicated mathematical operation with three numbers."""

    print( pow(numOne, numTwo) - numThree )
    
complexMathThing(4,5,2)
```

### Return Statements

In the example above, the function accomplished a task (printing something to the screen), but nothing produced by this function could be saved in a variable to be used later. If we want a function to produce a value (integer, float, string, ...) that can be saved in a variable, we need to include a `return` statement. This statement defines what value a function will produce.

Here, we redefine the function above to `return` the same value that it printed before.

```
def complexMathThing(numOne,numTwo,numThree):
    """A complicated mathematical operation with three numbers."""

    return pow(numOne, numTwo) - numThree

myVar = complexMathThing(4,5,2)
```

By adding a `return` statement, we can save a value produced by the function `complexMathThing()` in a variable (in this case, `myVar`)for future use.

```
print(myVar)
myVar * 3
```

### Optional Arguments

One way that we can make code more efficient, especially for functions that are used often and have lots of arguments, is to provide default values for some of the arguments. When we do this, a user only *has to* provide values for those arguments that don't have defaults - but they can also change the defaults for the others if they want to.

We can specify default values for arguments when we define the function. Below, we're redefining the `complexMathThing()` function from above, but providing default values for arguments `numTwo` and `numThree`.

```
def complexMathThing(numOne,numTwo=5,numThree=2):
    """A complicated mathematical operation with three numbers."""

    return pow(numOne, numTwo) - numThree
```

Now we can execute `complexMathThing()` by only providing a value for `numOne`, as long as we're ok with the default values for `numTwo` and `numThree`.

`complexMathThing(4)`

However, we can also run the function by providing new values for `numTwo` and/or `numThree`.

```
# Providing values for all three arguments
complexMathThing(4,6,1)

# Providing values for numOne and numTwo, so numThree uses its default of 2.
complexMathThing(4,6)
```

If we pass the function values for arguments in the same order as they're listed in the function definition, we don't have to write their names. But let's say that we wanted to provide values for `numOne` and `numThree`, but not `numTwo`. Then we'd have to provide the argument names, so the function "knows" to skip `numTwo`.

`complexMathThing(numOne=4,numThree=4)`

Note that we *always* have to provide a value for `numOne`, because it doesn't have a default. For example, what happens when you try this function call

`complexMathThing(numTwo=5,numThree=1)`

### Custom Functions Can Call Each Other

Well structured code will assign distinct tasks to separate functions, in order to keep the code readable. What this means is that you can have some custom functions that are called to do complex tasks, but within these functions you can call other custom functions to take care of smaller tasks. By structuring the code in this hierarchical way, large tasks are broken down into increasingly smaller ones.

```
# Here is code to take two nucleotide sequences and see if they have the same amino acid
# sequence.

genCode = {
    'ATA':'I', 'ATC':'I', 'ATT':'I', 'ATG':'M',
    'ACA':'T', 'ACC':'T', 'ACG':'T', 'ACT':'T',
    'AAC':'N', 'AAT':'N', 'AAA':'K', 'AAG':'K',
    'AGC':'S', 'AGT':'S', 'AGA':'R', 'AGG':'R',
    'CTA':'L', 'CTC':'L', 'CTG':'L', 'CTT':'L',
    'CCA':'P', 'CCC':'P', 'CCG':'P', 'CCT':'P',
    'CAC':'H', 'CAT':'H', 'CAA':'Q', 'CAG':'Q',
    'CGA':'R', 'CGC':'R', 'CGG':'R', 'CGT':'R',
    'GTA':'V', 'GTC':'V', 'GTG':'V', 'GTT':'V',
    'GCA':'A', 'GCC':'A', 'GCG':'A', 'GCT':'A',
    'GAC':'D', 'GAT':'D', 'GAA':'E', 'GAG':'E',
    'GGA':'G', 'GGC':'G', 'GGG':'G', 'GGT':'G',
    'TCA':'S', 'TCC':'S', 'TCG':'S', 'TCT':'S',
    'TTC':'F', 'TTT':'F', 'TTA':'L', 'TTG':'L',
    'TAC':'Y', 'TAT':'Y', 'TAA':'_', 'TAG':'_',
    'TGC':'C', 'TGT':'C', 'TGA':'_', 'TGG':'W'}

def nucToAA(nucSeq):
    """This function translates a DNA sequence into an amino acid sequence."""
    
    aaSeq = ""
    for i in range(len(nucSeq))[::3]:
        aaSeq = aaSeq + genCode[nucSeq.upper()[i:i+3]]
    return aaSeq
    
def testSameAA(nucSeqOne,nucSeqTwo,printAASeqs=False):
    """This function tests if two nucleotide sequences produce the same
       amino acid sequence."""
    
    aaSeqOne = nucToAA(nucSeqOne)
    aaSeqTwo = nucToAA(nucSeqTwo)
    if printAASeqs:
        print("First amino acid sequence: %s" % aaSeqOne)
        print("Second amino acid sequence: %s" % aaSeqTwo)
    return aaSeqOne == aaSeqTwo
    
testSameAA("ATAACAAACAGC","ATCACCAATAGT")

testSameAA("ATAACAAACAGC","ATCACCAATAGT",True)

testSameAA("ATAACAAACAGC","ATCACCAAAAGT",True)
```

### Lists as Arguments

Using a list as an argument for a function requires some extra thought. The value that's actually passed to the function when you include the list as an argument is the list's position in memory. This is called "passing by reference" and makes things a lot faster, because a program doesn't have to make a separate copy of all the values in the list to use inside the function. However, it means that **if you alter the list inside the function, its value will also be altered outside the function**.

```
myList = [1,2,3]

def addToList(listToModify,thingToAdd):
    """An example of passing a list as a function argument."""
    
    listToModify.append(thingToAdd)
    
print(myList)
addToList(myList,"dog")
print(myList)
```

Standard variables, like integers and floats, do not behave this way. Instead, they are "passed by value". That means if you alter the corresponding variable inside the function, the original variable remains unchanged.

```
myNum = 2

def addToNum(num):
    num = num + 2
    print(num)
    
addToNum(myNum)
print(myNum)
```

## Plotting in Python with `matplotlib`

### Jupyter Noteboook

Code corresponding to these notes about custom functions in Python can be found in the `Plotting.ipynb` Jupyter notebook in this repository.

### Additional Resources

- [Rougier's `matplotlib` Tutorial](https://github.com/rougier/matplotlib-tutorial)
- [`matplotlib` Homepage](https://matplotlib.org/index.html)
- [`matplotlib` Usage Guide](https://matplotlib.org/tutorials/introductory/usage.html)
- [`matplotlib` Gallery](https://matplotlib.org/gallery.html)

## Introduction

`Matplotlib` is a Python module that's commonly used for plotting. There are different ways to interact with `matplotlib`, but we'll be relying on the `pyplot` submodule. To import this module, it's helpful to give it a shorter "nickname"

`import matplotlib.pyplot as plt`

When a module (or submodule) is imported with the `as` keyword, it gives us a different (hopefully shorter) name with which to reference it.

Another module that's commonly used when analyzing and plotting data is `numpy`

`import numpy as np`

## Our First Plot

To generate some coordinates for plotting, we'll start by using a `numpy` function that creates a series of numbers on a regular interval

`x = np.linspace(1,10,0.25)`

Now, we'll create `y` values by first setting them equal to the `x` values (using a deep copy)

`y = copy.copy(x)`

and then adding some random noise

```
for i in range(len(y)):
    y[i] = y[i] + np.random.normal(0,0.5,1)
```

To create a simple line plot, we can use the `.plot()` function of `pyplot`

`plt.plot(x,y)`

![plot1](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot1.png)

Now, let's say we want to label our axes (which, as good scientists we should always do!). We can call the `.xlabel()` and `.ylabel()` functions from `pyplot` before we call `.plot()`.

```
plt.ylabel("Distance")
plt.xlabel("Day")
plt.plot(x,y)
```

![plot2](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot2.png)

Ok, this is looking better, but now we'd like to know how the observed trend compares to a line with slope 1 and intercept 0. We can add a second line to our plot with the x values on both axes.

```
plt.ylabel("Distance")
plt.xlabel("Day")
plt.plot(x,y)
plt.plot(x,x)
```

![plot3](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot3.png)

Note how `matplotlib` automagically changes the color of our 2nd line to be different than the first.

I actually really like these defaults, but let's say that we have a good reason that we want to make some changes. First, we want our 1:1 line underneath and we want it to be a blue, dashed line. For our data, we want to try red dots. `pyplot` let's us pass a third argument to `.plot()` that will adjust the color and style of plotting. The first letter (`b` or `r` here) will set the color and the next symbols (`o` or `--` here) change the plotting style.

```
plt.ylabel("Distance")
plt.xlabel("Day")
plt.plot(x,x,'b--')
plt.plot(x,y,'ro')
```

![plot4](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot4.png)

Here's a variant that uses a dotted 1:1 line.

```
plt.ylabel("Distance")
plt.xlabel("Day")
plt.plot(x,x,'b:')
plt.plot(x,y,'ro')
```

![plot5](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot5.png)

Ok, I actually prefer the solid 1:1 line, so let's go back to that, but let's make the line thicker.

```
plt.ylabel("Distance")
plt.xlabel("Day")
plt.plot(x,x,'b',linewidth=3)
plt.plot(x,y,'ro')
```

![plot6](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot6.png)

In many cases, we have multiple groups that we want to depict in a single scatter plot. In this case, we can use two `.plot()` calls back-to-back and `matplotlib` will color the two groups separately.

```
x_1 = np.random.normal(5,1,300)
y_1 = np.random.normal(5,1,300)

x_2 = np.random.normal(7,1,300)
y_2 = np.random.normal(7,1,300)

plt.plot(x_1,y_1,'o')
plt.plot(x_2,y_2,'o')
plt.xlabel("Trait 1")
plt.ylabel("Trait 2")
```

![plot7](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot7.png)

Because the two groups overlap some and the points are relatively large, it can be difficult to see some points. In this case, we may want to reduce the sizes of the points using the `markersize` argument. We can also make the symbols different between the groups to make them more distinctive.

```
x_1 = np.random.normal(5,1,300)
y_1 = np.random.normal(5,1,300)

x_2 = np.random.normal(7,1,300)
y_2 = np.random.normal(7,1,300)

plt.plot(x_1,y_1,'^',markersize=3)
plt.plot(x_2,y_2,'o',markersize=3)
plt.xlabel("Trait 1")
plt.ylabel("Trait 2")
```

![plot8](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot8.png)

## Multiple Plots

It is often convenient to show two or more plots together. Thankfully `pyplot` gives us an easy way to group plots.

```
# The plt.figure() function creates the overall plotting region
# The optional figsize argument allows us to adjust the plotting region's size

plt.figure(figsize=(10,4))

# The plt.subplot() function specifies where the first plot should go
# The first number indicates the number of rows
# The second number indicates the number of columns
# The third number indicates the position of the plot

plt.subplot(121)
plt.plot(x_1,y_1,'bo')

# We use the same dimensions (1 row, 2 columns) for the second plot, but 
# use position 2

plt.subplot(122)
plt.plot(x_2,y_2,'ro')

# After we've added the plots, we show the overall figure

plt.show()
```

![plot9](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot9.png)

By default, `pyplot` will match the minimum and maximum extent of each axis to the data being plotted. But in some cases, we'd like to have control over the axes to make plots equivalent.

```
plt.figure(figsize=(10,4))

plt.subplot(121)
plt.axis([0,11,0,11])  # Adding axis limits [xMin,xMax,yMin,yMax]
plt.plot(x_1,y_1,'bo')

plt.subplot(122)
plt.axis([0,11,0,11])
plt.plot(x_2,y_2,'ro')

plt.show()
```

![plot10](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot10.png)

## Categorical Variables

We don't always have two numerical coordinates for each data point. Sometimes, we have data that are naturally categorized in different groups and we're measuring one numeric value per group.

```
groups = ["groupOne","groupTwo","groupThree","groupFour"]     # Define names of groups
values = np.random.normal(10,2,4) # Drawing four numbers to act as values for groups
plt.bar(groups,values)
```

![plot11](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot11.png)

The `.plot()` function can still be used to plot values from categorical variables as a line plot. In this context, line plots make less sense, unless the groups have a natural ordering.

`plt.plot(groups,values,"purple",linewidth=3) # Using full word to specify color`

![plot12](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot12.png)

## Histograms

Histograms are useful for visualizing the frequencies of different numeric values in a group. Let's start with a simple example using random values drawn from a Normal distribution.

```
exampleNums = np.random.normal(0,2,500) # Example data

plt.hist(exampleNums,50)
plt.show()
```

![plot13](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot13.png)

We can also customize the appearance of histograms. Here, we've added gridlines, changed the color, changed the transparency (also known as alpha), and oriented the plot horizontally. We've also written the different arguments to the `.hist()` function on different lines, so that we can comment each of them individually.

```
plt.hist(exampleNums,               # Example data (the only required argument)
         50,                        # Number of bins
         color="orange",            # Color of the bars
         alpha=0.5,                 # Transparency of the bars (1=solid,0=totally transparent)
         orientation="horizontal")  # Orienting horizontally
plt.grid(True)
plt.show()
```

![plot14](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot14.png)

Here is an example with real data showing confirmed cases of COVID-19 across all US states and territories for 4/7/2020.

`stateCounts = [2197,211,2575,997,17540,5429,7781,928,1211,14739,9156,274,408,1210,13549,5507,1048,905,1289,16284,519,4371,15202,18852,1069,1915,3037,319,490,2101,747,44416,794,140081,3220,237,8,4782,1472,1181,14582,573,1229,2417,320,4010,8974,1752,575,45,3333,8682,412,2578,221]`

```
plt.hist(stateCounts,30)
plt.show()
```

![plot15](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot15.png)

You can clearly see how skewed the distribution of case counts is across the US, with nearly 140,000 cases in New York and more than 40,000 in New Jersey. Because the counts are so skewed, it may be helpful to change from using a linear scale of counts to a log scale. To do this, we need to do two things. We need to tell `pyplot` to change the x-axis to a log scale, which we do with `plt.scale("log")`. We also need to generate bins that are equally spaced on a log scale. Thankfully, `numpy` has a built-in function that can help us - `.logspace()` - which we use to pass the boundaries of our bins to `.hist()`.

```
plt.hist(stateCounts,
         bins=np.logspace(0,6,20))  # Using numpy to generate 20 values evenly spaced on a log
                                    # scale from 10^0 to 10^6. These values will define the 
                                    # boundaries of our histogram bins.
        
plt.xscale("log")              # Creating a log x-scale for our histogram

plt.grid(True,alpha=0.4)       # Adding grid and adjusting transparency

plt.show()                     # Displaying the histogram
```

![plot16](https://raw.githubusercontent.com/jembrown/CustomFunctions_Plotting_Week11_InProgress/master/images/plot16.png)

