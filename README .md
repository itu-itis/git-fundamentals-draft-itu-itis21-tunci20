
# Qapp(quiz application in console)

Qapp is a quiz application on console and it has general knowledge quiz and math quiz. It is used by single or multiple person. Answer the questions and show your success! 

## Tasks
• Creating questions by using lists and math operations.

• Being user-based and taking username from users before the quizzes.

• Storing data for each quiz in a file.

• Adding a timer for each quiz and storing with other analyses.

• Calculating a grade based on the answers to the questions and showing to the user the grade
and the time spend.

• Showing some analyses to user using the data of all the previous games of the user. They
contain:

• The average score of the user in all the quizzes played in the past, the highest score.

• Some statistics about the user or about the quiz game. To give an example, bar charts about the
statistics of the user.

• The time the user spent on each quiz.
## Instructions and their roles
For adding a timer we should import time library.
```python
import time
```
For assigning random numbers to math operations we can use random library.
  ```python
import random
```
For using csv files as database we have to import csv library.
  ```python
import csv
```
For preparing math questions we can use operator library.
  ```python
import operator
```
Also we have to use matplotlib and pandas libraries to plot graphs by using data.
  ```python
import matplotlib.pyplot as plt
import pandas as pd
```
So, we have added necessary libraries for the project.

Now, lets define a function for creating true or false questions. For this function we use empty list and we add our questions and answers to the list by using .append().

```python
def get_tof_statements():
    statements = []
    statements.append(["Magnet repel a piece of glass.", "F", "f"])
    statements.append(["Seismograph measure earthquake magnitude.", "T", "t"])
    statements.append(["Sound travels fastest in solids.", "T", "t"])
    statements.append(["Ukraine has mediterranean climate.", "F", "f"])
    statements.append(["Bears are resistant to the desert climate.", "F", "f"])
    statements.append(["Turkey is in the southern hemisphere.", "F", "f"])
    statements.append(["1 kg gold is heavier than 1 kg cotton.", "F", "f"])
    statements.append(["English is one of the Turkic languages.", "F", "f"])
    statements.append(["One year is 365 days.", "F", "f"])
    statements.append(["Tiger is a felin.", "T", "t"])
    return statements

```
And then, we define necessary functions for math questions. First one assign random operators and operands in addition to finding answer.
```python
def randomCalc():
    ops = {'+': operator.add,
           '-': operator.sub,
           '*': operator.mul }
    num1 = random.randint(0, 12)
    num2 = random.randint(1, 20)
    op = random.choice(list(ops.keys()))
    answer = ops.get(op)(num1, num2)
    print('What is {} {} {}?\n'.format(num1, op, num2))
    return answer
```
Second one compare answer and guess.
```python
def askQuestion():
    answer = randomCalc()
    guess = float(input())
    return guess == int(answer)
```
And then we add a simple menu for informing users.
```python
print("+--------------------------------+")
print("|       Welcome to Qapp!         |")
print("|       This is a quiz app       |")
print("|       Let's show yourself      |")
print("|For true false questions = 1    |")
print("|For math quiz = 2               |")
print("+--------------------------------+")
```
Also, taking user name and quiz type number is necessary to class data and store.
```python
user_name = input("Enter username: ")
type_number = int(input("Enter quiz number: "))
```
And we use if condition. If user want to choose true false quiz, 1 should be choosen. I added explains between instrucions so I don't think explaining repeatedly is necessary.
 ```python
  print('Welcome. This is a general knowledge quiz\n')
    #add a timer
    start_time = time.time()
    # Get true or false statements
    tof_statements = get_tof_statements()
    # Randomise tof statements
    random.shuffle(tof_statements)
    # Set player score to 0
    score = 0
    # Show tof statements using a loop
    for s in tof_statements:
        # Present statements
        print(s[0])
        # User enter guess
        guess = input("Enter T or F: ")
        # Check if guess is correct
        if guess == s[1] or guess == s[2]:
            print("Correct!")
            # Update score
            score = score + 1
        else:
            print("Incorrect!")
    print("Your quiz grade is: ", score * 10)
    end_time = time.time()
    duration = int(end_time - start_time)
    print("The time you spend(second): ", duration)
 ```
And then we add instrucions for math quiz under if condition.
```python
if type_number == 2:
    print('Welcome. This is a 10 question math quiz\n')
    #add a timer
    start_time = time.time()
    score = 0
    for i in range(10):
        correct = askQuestion()
        if correct:
            score += 1
            print('Correct!\n')
        else:
            print('Incorrect!\n')
    print("Your quiz grade is: ", (score * 10))
    end_time = time.time()
    duration = int(end_time - start_time)
    print("The time you spend(second): ", duration)
```
And our quiz application should have data and database. Also these data should can be examine by using graphs so, we add "with open" code and it will create csv file and it will store data about users in rows.
```python
with open('user_data.csv', "a") as user_data:
    user_writer = csv.writer(user_data, delimiter=',', quotechar='"', lineterminator = '\n')

    user_writer.writerow(['{}'.format(user_name), '{}'.format(score*10), '{}'.format(type_number), '{}'.format(duration)])
```
Of course, we add codes for plotting graphs by using data in file.
```python
data = pd.read_csv('user_data.csv')
df = pd.DataFrame(data)
#user names in the x axis 
X = list(df.iloc[:, 0])
#grades in the y axis
Y = list(df.iloc[:, 1])
ypoints = list(df.iloc[:, 3])

# Plot bar graph
plt.bar(X, Y, color='g')
plt.title("List of Scores                         (dashed lines indicate time)")
plt.xlabel("Users")
plt.ylabel("Grades")

# Plot line graph
plt.plot(ypoints, linestyle = 'dashed')

# Show the plot
plt.show()
```
Finally, we should show average grade.
```python
# write average
lst_grd = list(df.iloc[:, 1])
print("Average grade is: ", (sum(lst_grd)/len(lst_grd)))
```