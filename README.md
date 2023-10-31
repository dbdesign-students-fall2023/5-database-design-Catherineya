# Data Normalization and Entity-Relationship Diagramming

An assignment to normalize the structure of data and establish a set of Entity-Relationship Diagrams for the data.

## Original 1NF Data Set
| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

## Description of 1NF Data Set
This dataset is not compliant with 4NF for several reasons:
### Data Redundancy
There exists data redundancy in the original dataset, such as assignment topics, professors, classrooms, and related reading information, which are duplicated across multiple records. This redundancy results in storage inefficiencies and challenges in data maintenance.
### Not satisfy 4NF
The original dataset does not satisfy 4NF because it contains multiple candidate keys, such as assignment_id, student_id, professor, classroom, and professor_email. For example, professors assign identical homework topics to various sections of the course, each having different due dates, and readings are linked to specific assignments. This could result in insertion and deletion issues. Since it doesn't adhere to 2NF and does not adhere to 4NF. 
### Update Exception
When multiple students complete the same assignment in the same classroom under the same professor, updating the professor, classroom, or assignment details requires redundant update operations, potentially leading to update errors. Furthermore, if there's any data inconsistency, like a classroom information change, updates must be applied to multiple records, increasing the risk of data inconsistencies.

Therefore, we made some changes to the original dataset to make it 4NF-compliant, which is shown below.
## Tables containing the 4NF-compliant version of the data set
### Professor
| professor | professor_email |
| :-------- | :-------------- |
| Melvin    | l.melvin@foo.edu  |
| Logston   | e.logston@foo.edu |
| Nevarez   | i.nevarez@foo.edu |
| ...       | ...             |

### Assignment
| assignment_id | assignment_topic | relevant_reading | due_date | classroom |
| :------------ | :--------------- | :--------------- | :------- | :-------- |
| 1             | Data normalization | Deumlich Chapter 3 | 23.02.21 | WWH 101   |
| 2             | Single table queries | Dümmlers Chapter 11 | 18.11.21 | 60FA 314  |
| 5             | Python and pandas | Dümmlers Chapter 14 | 05.05.21 | 60FA 314  |
| 4             | Spreadsheet aggregate functions | Zehnder Page 87 | 04.07.21 | WWH 201   |
| ...           | ...              | ...              | ...      | ...      | 

### Assignment_Classroom
| assignment_id | classroom |
| :------------ | :-------- |
| 1             | WWH 101   |
| 2             | 60FA 314  |
| 5             | 60FA 314  |
| 4             | WWH 201   |
| ...           | ...       |

### Due_Date
| assignment_id | due_date |
| :------------ | :------- |
| 1             | 23.02.21 |
| 2             | 18.11.21 |
| 5             | 05.05.21 |
| 4             | 04.07.21 |
| ...           | ...      |

### Assignment_Topic
| assignment_id | assignment_topic |
| :------------ | :--------------- |
| 1             | Data normalization |
| 2             | Single table queries |
| 5             | Python and pandas |
| 4             | Spreadsheet aggregate functions |
| ...           | ...              |



### Reading 
| professor | relevant_reading |
| :-------- | :--------------- |
| Melvin    | Deumlich Chapter 3 |
| Logston   | Dümmlers Chapter 11 |
| Logston   | Dümmlers Chapter 14 |
| Nevarez   | Zehnder Page 87 |
| ...       | ...              |



### student_grade
| assignment_id | student_id | grade |
| :------------ | :--------- | :---- |
| 1             | 1          | 80    |
| 2             | 7          | 25    |
| 1             | 4          | 75    |
| 5             | 2          | 92    |
| 4             | 2          | 65    |
| ...           | ...        | ...   |

### Assignment_Professor
| assignment_id | professor |
| :------------ | :-------- |
| 1             | Melvin    |
| 2             | Logston   |
| 5             | Logston   |
| 4             | Nevarez   |
| ...           | ...       |


## Description of 4NF Data Set and Changes
I consider the assignment_id, professor, student_id and the relevant_reading as the primary key to split the dataset into multiple tables in order to solve the problems such as the data redundancy and so on. And the explanation of each table is shown below.
### 1 Professor
The professor table contains the professor and professor_email fields. And in this table, the professor field is the primary key. As the professor email is only dependent on the professor and each professor has only one email address, the professor table is in 4NF.

### 2 Assignment
The assignment table contains the assignment_id, assignment_topic, relevant_reading, due_date and classroom fields. And in this table, the assignment_id is the primary key and the assignment_topic, relevant_reading, due_date and classroom fields are only dependent on the assignment_id. Also, the assignment_id(sections) here I consider here represents different assignments that with different topics, different due dates, different relevant readings and assigned in different classrooms. Therefore, the assignment_id is unique and can decide the other fields. Therefore, the Assignment table is in 4NF. 

### 3 Assignment_Classroom
The Assignment_Classroom table contains the assignment_id and classroom fields. And in this table, the assignnment_id is the primary key and the classroom field is only dependent on the assignment_id. As each assignent_id(section) can only be assigned in one classroom, each classroom can have lots of assignment_ids(sections), so the Assignment_Classroom table is in 4NF.

### 4 Due_Date
The due_date table contains the assignment_id and due_date fields. The assignment_id is the primary key. And the due_date field is only dependent on the assignment_id. As each assignment_id(section) can only have one due date, each due date can have lots of assignment_ids(different sections), so the due_date table is in 4NF.

### 5 Assignment_Topic
The assignment_topic table contains the assignment_id and assignment_topic fields. The assignment_id is the primary key. And the assignment_topic field is only dependent on the assignment_id. As each assignment_id(section) can only have one topic, each topic can relate to lots of assignment_ids(sections), so the assignment_topic table is in 4NF.

### 6 Reading
The reading table contains the professor and relevant_reading fields. Here, we use a composite primary key(professor, relevant_reading) to represent the primary key. As each professor can give different relevant readings, each reading can also be given by different professors. But a assignment can only have one combination of relevant_readings, while a reading can be given to different assignments. Therefore, the reading table is in 4NF.

### 7 student_grade
The student_grade table contains the assignment_id, student_id and grade fields. The assignment_id and student_id fields are the primary key of the student_grade table. This table represents which student gets which grade in which assignment. A student can get multiple grades in different assignments with different assignment_ids, and a assigment (one assignment_id) can be completed and graded by different students. The student_grade table is in 4NF.

### 8 Assignment_Professor
The Assignment_Professor table contains the assignment_id and professor fields. The assignment_id is the primary key of the Assignment_Professor table. This table represents which professor gives which assignment. A professor can give multiple assignments with different assignment_ids, and a assignment (one assignment_id) can only be given by one professor. The Assignment_Professor table is in 4NF.

## Entity-Relationship Diagrams
![ER Diagram](./images/example-er-diagrams-Basic%20shapes.drawio.svg)

I discussed this assignment with my classmate Haotong Wu(hw2933). 