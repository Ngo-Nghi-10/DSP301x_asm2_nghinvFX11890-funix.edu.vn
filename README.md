# ASSIGNMENT 2 

## TASK1
Write a program that allows the user to enter the name of a file. Try to open the file provided for read access. If the file exists, you can print a confirmation message. If the file does not exist, you should let the user know that the file cannot be found and prompt them again. 
- _Existing files are class1.txt, class2.txt.......class8.txt_
 * Input the name of file you want to open
```python
file_name = input('Enter a class file to grade (i.e. class1 for class1.txt): ')
```
 * Write a function to check and print a confirmation message
```python
def checkfile(file):
    data_file = ['class'+ str(i) for i in range(1,9)]
    try:
        if file in data_file:
            print('Successfully opened',file+'.txt','\n')
        else:
            print('File cannot be found.')
    except:
        print('Error')
```
* The confirmation are the follwing statements below:

        - Enter a class file to grade (i.e. class1 for class1.txt): class1
        - Successfully opened class1.txt
        - OR
        - Enter a class file to grade (i.e. class1 for class1.txt): class9
        - File cannot be found.


## TASK 2

In task 2, you need to:
*Report the total number of lines of data stored in the file.
*Parse each line and make sure it is "valid".
    -A valid line containing a list of 26 comma-separated values
    -N# for a student is the first item on the line. It must contain the character “N” followed by 8 numeric characters.
    -If a data stream is invalid, you should report it to the user by printing an error message. You should also count the total number of valid data lines in the file.
