# Functional Programming
Any programing languages which supports pure functions and immutable values. <br>

# Pure Function

**Characteristics:**
- The input solely determines the output.
- The function deosn't changes it's input value.
- There are no side effects, i.e. the function only does what it is intended to do. It doesn't do anything extra.

E.g: Dollars to Rupeees.
dollarsToRs(40) => could give 2800 today and 2900 tomorrow, so it depends on something external. Hence it is not a pure function.

Syntactic Examples
```scala
def func1(a:Int) = {a = a+1; return a} //this is an impure function
def func1(a:Int) = {return a+1} // is a pure function as we are not altering it's input.

//Another example
var a=5;
def func(i:Int):Int = {return a+i} // impure func as it is dependent on a.

def func(i:Int, a:Int):Int = {return a+i} // pure func as there are no external dependencies.

def func2(a:int) = {println("hello); return a} 
/*impure function because of the side effect, i.e. printing a string
By removing this print statement, the function becomes pure func.
*/

def func2(a:Int) = {return a} //pure function.
```
# Referencial Transparency: A way to test the purity of the function
A func is referentially transparent, if replacing the function with a value do not impact the result.

sqrt(4) = 2. So where ever we have sqrt function we replace it by 2 and the value of the resultant func doesn't change. That's referential transparency.

# First Class functions

It should support 3 features to be a first class function:

- We should be able to assign a function to a variable
- ```scala
  def doubler(i:Int): Int = {
    return i*2
  }
  ```
var a = doubler(_) //assigning a function to a variable.
a(3) // 6

- we should be able to pass a function as a parameter to another function.
  ```scala
  def tripler(a:Int):Int = {a*3}

  def func(i:Int, f:Int => Int): Int = {f(i)}
  func(5, tripler) //15
    ```

- we should be able to return a function from a function.
```scala
def func = {x:Int => x * x} //a function which returns a function
func(4) //16
```

# Higher order functions
A function which either takes a function as an input parameter or returns another function as its output.

- Map:
  if we have n inputs rows, then we will get n output rows also.
```scala
var a = 1 to 10    

def doubler(i:Int):Int = {i*2}

a.map(doubler)  //  scala.collection.immutable.IndexedSeq[Int] = Vector(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)
```

# Anonymous Function
A function with out a name.

```scala
var a = 1 to 10

def doubler(i:Int):Int = {i * 2}

a.map(doubler)

//rather than defining the function, we can create an anonymous function

a.map(x => x * 2)  // scala.collection.immutable.IndexedSeq[Int] = Vector(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)
```

# Immutability
we cannot change the value of the variables. 

var - variable that can be mutated.
val - variable that can not be mutated.

val is preferred as it is immutable and doesn't have any side effects.

# Loops vs Recursion vs Tailed Recursion
Code to write a factorial of a function

```scala
//Factorial using For loop!
def factorial(input:Int):Int = {
    var result : Int = 1
    for (i <- 1 to input) {
        result = result * i
    }
    result
}

//Factorial using recursion
def factorialRec(input:Int):Int = {
    if(input == 1) 1
    else input * factorial(input-1)
}

//Factorial using Tail Recursion - has memory improvements on normal recursion
def factorialTailRec(input:Int, result:Int): Int = {
    if (input == 1) result
    else factorialTailRec(input-1, result*input)
} // the recursive call should be the last statement in the function
```

# Statement vs Expression
- Each line in a code block is a statement
- Expression is a line of code that returns something

In scala, we don't have statements, we have only expressions. Each statement returns something.
```scala
val a = println("hello world")

//a contains Unit() as print statement returns unit.

val a = 5
val x = if (a == 5) "true" else "false"
```