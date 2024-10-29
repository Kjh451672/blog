# Daily Dose of Debugging

 When you hear the term "debugging", you could take it pretty literal and pretty metaphorical. But in actuality, debugging is just a term that programmers use when there is an error in the code that causes it to either not work, or not run as intended. But before I get into my experience with debugging, lets get into how this whole term sprung up in the first place.

 ## History of "Debugging"

<img src="/blog/images/moth_debugging.png" height="350">

 The term of "bug" or "debugging" first came to be, when computers were still a very new concept to the world. The use of the term gor widespread in 1945, but was first ever used by Thomas Edison in 1878. One day, Thomas Edison had some toruble with his personal computer. After inspecting it, he found a literal bug crawling around the inside of his computer, messing with the curciutry. Hence, the term "debugging" was born.

## Debugging: Example #1

To test our skills on debugging, our teacher gave us various snippets of code with a minimalamount of errors, that caused the code to either crash, or not work as intended. This first snippet of code was an example he made for us to give us an idea oon what to fix and what to describe.

```python
temperature = 75

if temperature > 80:
    print("It's hot")
elif temperature > 50:
    print("It's temperate")
elif temperature < 0:
    print("It's cold")
```

The expected purpose of this code is to determine whether it is cold or hot, depending on the value of the temperature. However, if you take a closer look at the code, you may realize that is does not work as intended, as it doesnt account for number between 0 and 50.

```python
#Added = sign to the first elif statement, and changed the second elif statment to an else statement 
if temperature > 80:
    print("It's hot")
elif temperature >= 50:
    print("It's temperate")
else :
    print("It's cold")
```

This is where debugging comes into play. Compared to the code snippet above, it was all a matter of changing the conditional statement of the first elif-statement, and removing the second elif-statement. By changing the final elif to an else statement, and changing the first elif's conditions to where "temperature >= 50", this will make sure any temperature less than 50 is determined to be cold. This example, much like the other examples, have logic errors in their code snippets.

## Debugging: Example #2

```python
text = "Hello, world, my name is"
count = 0

for char in text:
    if char == "":
       count += 1

print(count)
```

The expected purpose of this code is to count the amount of spaces in a given string. However in line 5, the if-statement checking for said spaces doesn't work. The reason is because "" is not the same as " ". The if statement is incorrect because it technically checks for nothing, so the value of count never increases.

```python
text = "Hello, world, my name is"
count = 0

#Change "" to " " so it properly reads spaces
for char in text:
    if char == " ":
       count += 1
print(count)
```

To fix this code, all I havd to do was change the closed quotation marks to an open one, so it actually reads the open spaces in the string. Now this for-loop will identify all the spaces in a given string. Whenever you're writing code, it's always best to double check what youve written since the computer will also miss things that you don't account for.

## Debugging: Example #3

 ```python
 print("give me a number")
n = input()

for num in range(1, n):
    if num % 2 < 0:
        print(num, "is even.")
    else:
        print(num, "is odd.")
 ```

 This next snippet of code gets more involved with the user, as they require them to input any number they want. Once they do, the for-loop prints out all of the numbers in that range, along with a print statement stating whether that number is even or odd. There are several problems with this snippet of code.

 The first part of the if-statement doesn't work properly since it is only true when there is a remainder and false when it isn't. Odd numbers would be read as even and vice versa. The nvariable is also declared improperly, as it's defined as a string. Also, whenever range is used, the max number is always one short than what is used, meaing when the user inputs a number, the range will be one less than it should be. 

 ```python
 print("give me a number")
 #Declares n as an integer
n = int(input())

#Changed less than sign to ==, set n = to int(input()), and had range set to n + 1
for num in range(1, n + 1):
    if num % 2 == 0:
        print(num, "is even.")
    else:
        print(num, "is odd.")
 ```

What I did to fix these bugs, was change the greater than sign to a double equal sign, so the code properly reads for 0, and change n = int(input()), and in range have n + 1. This way, n is registered as an integr, the range function accounts for all the numbers the user wants, and the if statement now properly checks to see what numbers ar even or odd.

## Debugging: Example #4

```python
num = int(input("Enter an integer: "))

if num < -1:
  print("No negative numbers.")
else:
  result = 1
  for i in range(1, num):
    result *= i   

  print("Factorial of " + num + "is" + result)
```
The purpose of this code snippet is to calculate the factoral of a given number. There are also several problems with this code. The range in the conditional is off by 1 as it will do 1 less than the imputed range, and the print statement will result in a TypeError, as the variables used in it are not properly concatenated.

```python
num = int(input("Enter an integer: "))
#Changed ranged to num + 1, and add str() to all variables in end print statement
if num < -1:
  print("No negative numbers.")
else:
  result = 1
  for i in range(1, num + 1):
    result *= i   
  print("Factorial of " + str(num) + " is " + str(result))
```
To fix this problem, I changed the conditional for range to (1, num + 1), and add str() to the variables in the final print statement. Now it correctly prints out the result in the proper format, without any errors occuring.

## Debugging: Example #5

```python
attempts = 0
correct_password = "secret"

while True:
    password = input("Enter your password: ")
    attempts += 1

    if password == "incorrect_password":
        print("Correct password!")
    else:
        print("Incorrect password")

    if attempts > 3:
        print("Too many attempts")
        break
```
The purpose of this final code snippet was to ask the user to enter in a password, and have them guess it in 3 attempts. Compared to the other snippets of code, this one had the most bugs.

The attempts variable is in the wrong spot, and the conditional for the if-statement is incorrect since it should be checking if password is equal to corect_password. There isn't a break statement if the correct password is guessed, and the attempts variable for the “too many attempts”, and it lets the user take more than 3 attempts to guess the password.

```python
attempts = 1
correct_password = "secret"
#Added a break statement if password is correctly guessed, moved attempts variable to end of if-statement 
#set attempts = 1, changed condition to if attempts ==3, and chnged if-statement to have password = correct password
while True:
    password = input("Enter your password: ")
    if password == correct_password:
        print("Correct password!")
        break
    else:
        print("Incorrect password")
    if attempts == 3:
        print("Too many attempts")
        break
    attempts += 1
```

To fix this, I added a break statement into the if-statement if the password is guessed correctly. I also moved the attempts variable to the end of the while-loop, so that it increments after every iteration. I set the number of attempts to a base value of one, since it only increases at the end of each guess, adn finally, I changed the first if-statement's conditional so it properly checks for the correct password.