# ASSIGNMENT 2 

## TASK1
Write a program that allows the user to enter the name of a file. Try to open the file provided for read access. If the file exists, you can print a confirmation message. If the file does not exist, you should let the user know that the file cannot be found and prompt them again. 
- _Existing files are class1.txt, class2.txt.......class8.txt_
 * Input the name of file you want to check and run the function checkfile in task 1 to make sure that the file is 'valid':
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

        Enter a class file to grade (i.e. class1 for class1.txt): class1
        Successfully opened class1.txt
        OR
        Enter a class file to grade (i.e. class1 for class1.txt): class9
        File cannot be found.


## TASK 2

In task 2, you need to:
* Report the total number of lines of data stored in the file.
* Parse each line and make sure it is "valid".
    - A valid line containing a list of 26 comma-separated values
    - N# for a student is the first item on the line. It must contain the character “N” followed by 8 numeric characters.
* If a data stream is invalid, you should report it to the user by printing an error message. You should also count the total number of valid data lines in the file.

### Code
* Input the name of file you want to open
```python
file_name = input('Enter a class file to grade (i.e. class1 for class1.txt): ')
checkfile(file_name)
```
* Write a function invalid_row(file_name) to:
    * count the number of rows is 'valid' and 'invalid'
    * If invalid => print the invalid rows with information for each row: 'Invalid line of data: N# is invalid' or Invalid line of data: does not contain exactly 26 values
```python
def invalid_rows(file_name):
    try:
        #open the file_name
        file_class=file_name+'.txt'
        with open(file_class,'r') as read_file:
            text_lines=read_file.readlines()
        
        print('**** ANALYZING **** \n')
        
        N_valid=0
        N_invalid=0
        for i in text_lines:
            text=i.split(',')
            text_values=np.array(text)

            #Invalid line of data: does not contain exactly 26 values
            if len(text_values) != 26:
                N_invalid = N_invalid + 1
                print('Invalid line of data: does not contain exactly 26 values:')
                print(i)

            #Invalid line of data: N# is invalid
            a=re.findall('N[0-9]{8}',text[0])
            if text[0] not in a:
                N_invalid = N_invalid + 1
                print('Invalid line of data: N# is invalid')
                print(i)

        N_valid=len(text_lines)- N_invalid

        #Print results on screen
        if N_invalid==0:
            print('No errors found! \n')
        print('**** REPORT **** \n')
        print('Total valid lines of data:',N_valid)
        print('Total invalid lines of data:',N_invalid)
    except:
        pass
invalid_rows(file_name)
```
* The results for file_name='class2' after run the code above is:

        Enter a class to grade (i.e. class1 for class1.txt): class2
        Successfully opened class2.txt 

        **** ANALYZING **** 

        Invalid line of data: does not contain exactly 26 values:
        N00000023,,A,D,D,C,B,D,A,C,C,,C,,B,A,C,B,D,A,C,A,A

        Invalid line of data: N# is invalid
        N0000002,B,A,D,D,C,B,D,A,C,D,D,D,A,,A,C,D,,A,C,A,A,B,D,D

        Invalid line of data: N# is invalid
        NA0000027,B,A,D,D,,B,,A,C,B,D,B,A,,A,C,B,D,A,,A,A,B,D,D

        Invalid line of data: does not contain exactly 26 values:
        N00000035,B,A,D,D,B,B,,A,C,,D,B,A,B,A,A,B,D,A,C,A,C,B,D,D,A,A

        **** REPORT **** 

        Total valid lines of data: 21
        Total invalid lines of data: 4

## TASK 3

* you will write a program to grade exams for a given section. The exam consists of 25 questions. Here is a string representing the answers:
        answer_key = "B,A,D,D,C,B,D,A,C,C,D,B,A,B,A,C,B,D,A,C,A,A,B,D ,D"

* Your program should use these responses to calculate a score for each valid line of data. Points can be calculated as follows:

            * +4 points for each correct answer
            * 0 points for each answer ignored
            * -1 point for each wrong answer
* You will also want to calculate the following statistics for the entire class:

            * Medium score
            * Highest point
            * Lowest score
            * Score range (highest minus lowest)
            * Median value


### CODE
* Input the file_name and run the funtions to make sure the file_name is valid. Then run the function invalid_row in task 2 to check the invalid row in each file.
```python
file_name = input('Enter a class to grade (i.e. class1 for class1.txt): ')
checkfile(file_name)
invalid_rows(file_name)
```

* Create a Functions prepare(file_name) to transfer all ID number and answers into DataFrame by using Pandas:
*
     - Delete all the invalid row ( the rơ with the length not equals with 26 and ID number is not contain “N” followed by 8 numeric characters
     - Transfer all the row into DataFrame using Pandas: pd.DataFrame()
```python
#Function to create DataFrame of valid rows for entire class.  
def prepare(file_name):
    with open(file_name+'.txt','r') as r:
        text_lines=r.readlines()
        
        #Delete invalid rows 
        text=[]
        for i in range(0,len(text_lines)):
            row=text_lines[i]
            line=row.split(',')
            a=re.findall('N[0-9]{8}',line[0])
            if len(line)!=26:
                pass
            elif line[0] in a:
                text.append(row)
        row_data=[i.split(',') for i in text]
    data=pd.DataFrame(row_data)
    return data
```

* Create Function clean(data) to clean data in DataFrame and caculate grades for each student, also prepare data for answering the questions:

```python
def clean(data_prepare):
    data=prepare(file_name)
    
    #change the columns's name
    header=['Q'+str(i) for i in range(0,26)]
    data.columns=header
    data=data.rename(columns={'Q0':'student_number'})
    data['Q25']=data['Q25'].str.replace('\n','')
    
    #answer keys for each question
    answer_key = "B,A,D,D,C,B,D,A,C,C,D,B,A,B,A,C,B,D,A,C,A,A,B,D,D"
    answer_key=answer_key.split(',')
    answer=pd.DataFrame(answer_key)

    data['Point']=0

    #caculate points for each student
    for i in range(len(data)):
        for j in range(1,26):
            if data.iloc[i,j]==answer.iloc[j-1,0]:
                data.iloc[i,26]=data.iloc[i,26]+4
            elif data.iloc[i,j]== '':
                data.iloc[i,26]=data.iloc[i,26]+0
            elif data.iloc[i,j] != answer.iloc[j-1,0]:
                data.iloc[i,26] = data.iloc[i,26] -1

    return data
```
* Finally, print answers the questions:
```python
#Answer the questions 
data=clean(data_prepare)
range_score=data['Point'].max()-data['Point'].min()
print('Mean (average) score:',round(data['Point'].mean(),2))
print('Highest score:',data['Point'].max())
print('Lowest score:',data['Point'].min())
print('Range of scores:', range_score)
print('Median score:',data['Point'].median())

```
Here is answering for question after run the code above with class2


    Enter a class to grade (i.e. class1 for class1.txt): class2
    Successfully opened class2.txt 

    **** ANALYZING **** 

    Invalid line of data: does not contain exactly 26 values:
    N00000023,,A,D,D,C,B,D,A,C,C,,C,,B,A,C,B,D,A,C,A,A

    Invalid line of data: N# is invalid
    N0000002,B,A,D,D,C,B,D,A,C,D,D,D,A,,A,C,D,,A,C,A,A,B,D,D

    Invalid line of data: N# is invalid
    NA0000027,B,A,D,D,,B,,A,C,B,D,B,A,,A,C,B,D,A,,A,A,B,D,D

    Invalid line of data: does not contain exactly 26 values:
    N00000035,B,A,D,D,B,B,,A,C,,D,B,A,B,A,A,B,D,A,C,A,C,B,D,D,A,A

    **** REPORT **** 

    Total valid lines of data: 21
    Total invalid lines of data: 4
    Mean (average) score: 78.0
    Highest score: 100
    Lowest score: 66
    Range of scores: 34
    Median score: 76.0

* We also can check the DataFrame with: data.head() 
```python
data.head()
```
	student_number	Q1	Q2	Q3	Q4	Q5	Q6	Q7	Q8	Q9	...	Q17	Q18	Q19	Q20	Q21	Q22	Q23	Q24	Q25	Point
0	N00000021	B	A	C	D	C	C	D	D	C	...	B		A	D	A	A	B	D		68
1	N00000022	B	A	D	A	C	B	D	D	C	...	B	A	A	C	A	A	B	A	D	76
2	N00000024	C	C	D	D	C	B		A	C	...	B	D	A	C	A	B	B		D	73
3	N00000026	B	C	D	D	C	D	B	A	C	...	B	D		C	A	A	B	D	D	72
4	N00000028	B	A	D	D	D	B	D	A	C	...	B	D	C	C	A	A	B	C	D	73
5 rows × 27 columns
