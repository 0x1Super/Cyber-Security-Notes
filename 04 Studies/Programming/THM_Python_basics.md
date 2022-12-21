# Basics
![[Pasted image 20210920083321.png]]
![[Pasted image 20210920083334.png]]
String - Used for combinations of characters, such as letters or symbols
Integer - Whole numbers
Float - Numbers that contain decimal points or for fractions
Boolean - Used for data that is restricted to True or False options
List - Series of different data types stored in a collection
# Logical Operation
Logical Operation	Operator	Example
Equivalence          	   ==	        if x == 5
Less than	                 <          	if x < 5
Less than or equal to	<=	       if x <= 5
Greater than             	>	         if x > 5
Greater than or equal to	>=	 if x >= 5
# Boolean Operation
![[Pasted image 20210920083546.png]]
![[Pasted image 20210920083604.png]]
# IF 
The if keyword indicates the beginning of the if statement, followed by a set of conditions.
The if statement is only run if the condition (or sets of conditions) is true. In our example, it's age < 17; if that condition is true (age is above 17), the code within the if statement runs. Per the example, if certain conditions are not met, the program can default to running code shown in the else part of the if statement. 
A colon : marks the end of the if statement.
Note the indentation. Anything after the colon that is indented, is considered part of the if statement, which the program will execute.
![[Pasted image 20210920083640.png]]

# While Loops
 loops allow programs to iterate and perform actions a number of times. There are two types of loops, for and while loops.



Let's begin by looking at how we structure a while loop. We can have the loop run indefinitely or (similar to an if statement) determine how many times the loop should run based on a condition.

i = 1
while i <= 10:
     print(i)
     i = i + 1
This while loop will run 10 times, outputting the value of the i variable each time it iterates (loops). Let's break this down:

- The i variable is set to 1
- The while statement specifies where the start of the loop should begin
- Every time it loops, it will start at the top (outputting the value of i)
- Then it goes to the next line in the loop, which increases the value of i by 1
- Then (as there is no more code for the program to execute), it goes to the top of the loop, starting the process over again
- The program will keep on looping until the value of the i variable is greater than 10

# For Loops
A for loop is used to iterate over a sequence such as a list. Lists are used to store multiple items in a single variable, and are created using square brackets (see below). Let's learn through the following example:

websites = ["facebook.com", "google.com", "amazon.com"]
for site in websites:
     print(site)
This for loop shown in the code block above, will run 3 times, outputting each website in the list. Let's break this down:

- The list variable called websites is storing 3 elements
- The loop iterates through each element, printing out the element
- The program stops looping when it's been through each element in the loop
- To give a real-world scenario, you could create a program that checks if a website is online or if an item is in stock. You would loop through the website list,  add functionality inside the loop to check the website, and output the results. The "Python for Pentesters" room shows you how to use Python to enumerate a target, build a keylogger, scan a network, and more.

In Python, we can also iterate through a range of numbers using the range function. Below is some example Python code that will print the numbers from 0 to 4. In programming, 0 is often the starting number, so counting to 5 is 0 to 4 (but has 5 numbers: 0, 1, 2, 3, and 4)

for i in range(5):
     print(i)
Result
0
1
2
3
4

# Functions 
As programs start to get bigger and more complex, some of your code will be repetitive, writing the same code to do the same calculations, and this is where functions come in. A function is a block of code that can be called at different places in your program.

You could have a function to work out a calculation such as the distance between two points on a map or output formatted text based on certain conditions. Having functions removes repetitive code, as the function's purpose can be used multiple times throughout a program.
def sayHello(name):
     print("Hello " + name + "! Nice to meet you.")

sayHello("ben") # Output is: Hello Ben! Nice to meet you

- The def keyword indicates the beginning of a function. The function is followed by a name that the programmer defines (and is a function parameter). In our example, it's sayHello.
- Following the function name is a pair of parenthesis () that holds input values, data that we can pass into the function. In our example, it's a name.
- A colon : marks the end of the function header.

# Files

In Python, you can read and write from files. We've seen that in cyber security, it's common to write a script and import or export it from a file; whether that be as a way to store the output of your script or to import a list of 100's of websites from a file to enumerate. Let's dive straight into an example:

f = open("file_name", "r")
print(f.read()) *Result will be to read what is in the file*
To open the file, we use the built-in open() function, and the "r" parameter stands for "read" and is used as we're reading the contents of the file. The variable has a read() method for reading the contents of the file. You can also use the readlines() method and loop over each line in the file; useful if you have a list where each item is on a new line. In the example above, the file is in the same folder as the Python script; if it were elsewhere, you would need to specify the full path of the file.

You can also create and write files. If you're writing to an existing file, you open the file first and use the "a" in the open function after the filename call (which stands for append). If you're writing to a new file, you use "w" (write) instead of "a". See the examples below for clarity:
f = open("demofile1.txt", "a") # Append to an existing file
f.write("The file will include more text..")
f.close()

f = open("demofile2.txt", "w") # Creating and writing to a new file
f.write("demofile2 file created, with this content in!")
f.close()

# Imports
In Python, we can import libraries, which are a collection of files that contain functions. Think of importing a library as importing functions you can use that have been already written for you. For example, there is a "date" library that gives you access to hundreds of different functions for anything date and time-related.

import datetime
current_time = datetime.datetime.now()
print(current_time)
We import other libraries using the import keyword. Then in Python, we use that import's library name to reference its functions. In the example above, we import datetime, then access the .now() method by calling library_name.method_name(). Copy and paste the example above into the code editor.
