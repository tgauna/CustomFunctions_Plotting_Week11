# Custom Functions and Plotting in Python

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
