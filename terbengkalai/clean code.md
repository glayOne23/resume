## Chapters:
1. Clean Code
2. Meaningful Names
3. Functions
4. Comments
5. Formatting
6. Object and Data Structure
7. Error Handling
8. Boundaries
9. Unit Test
10. Classes
11. System
12. Emergence
13. Concurrency 
14. Successive Refinement
15. JUnit Internals
16. Refactoring SerialDate
17. Smell and Heuristic
---
## This book divided into 3 big part:
1. The first several chapters describe the principles, patterns, and practice of writing clean code
2. Consists of several case studies of ever-increasing complexity. Each case study is an exercise in cleaning up some code—of
transforming code that has some problems into code that has fewer problems.
3. Payoff. It is a single chapter containing a list of heuristics and smells gathered while creating the case studies.
---
## What clean code means by Bjarne Founder of C++:
1. elegant
2. efficience
3. error haldling should be completed
4. the dependencies minimal to ease maintenance
5. The logic should be straightforward to make it hard for bugs to hide
---
## Chapter 1 : Meaningful Names

### 1. Use Intention-Revealing Names
- The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.
    - exp:
        - bad name: 
            ``` java 
            int d; //elapsed time in day 
            ```
        - good name : 
            ``` java
            int elapsedDay;
            ```
### 2. Avoid Disinformation
- example to describe group of city, dont name it ``` cityList ``` unless it’s actually a List (name of data structure in programming language), use cityGroup instead

### 3. Make Meaningful Distinction (Perbedaan yang Jelas)
- usahain pake nama konfensional untuk menamai, class, function, atau variabel
### 4. Use Pronounceable (dapat diucapkan) Names
- exp : gendymdhs(stand for generation date, year, month, day, hour, second) can be replaced with genTimestamp 
### 5. Use Searchable Names
- Karena akan sulit menemukan fungsi dengan nama 'a()' dibanding dengan nama 'createUser()'
### 6. Avoid Encodings -missed-
- remember containers of variables can change
- Exp : phoneString, paymentInt are bad name , payment can be made float in future.
### 7. Hungarian Notation
### 8. Member Prefixes
- Di java, dikenal namanya prefix (awalan nama). Misal variable dengan diawali huruf m_ (m_desc)merupakan non-public and non-static field name, saat ini kita sebaiknya tidak menggunakan itu lagi. 
- Kenapa? karena  orang dengan cepat belajar untuk mengabaikan awalan (atau akhiran) untuk melihat yang bermakna bagian dari nama. Semakin banyak kita membaca kode, semakin sedikit kita melihat awalan. Akhirnya awalan menjadi kekacauan yang tak terlihat dan penanda kode yang lebih lama.
### 9. Interface and Implementation -missed-
### 10. Avoid Mental Mapping
- Menggunakan nama yang mereka pikir nama dengan maksud lain
### 11. Class Names
- Classes and objects should have noun or noun phrase names like Customer, WikiPage, Account, and AddressParser
- A class name should not be a verb
### 12. Method Names
- Methods should have verb or verb phrase names like postPayment , deletePage , or save .
- Accessors, mutators, and predicates should be named for their value and prefixed with 'get' , 'set' , and 'is' according to the javabean standard.
    ``` java
    string name = employee.getName();
    customer.setName("mike");
    if (paycheck.isPosted())...
    ```
### 13. Dont be Cute
- Will they know what the function named HolyHandGrenade is supposed to do? Sure, it’s cute, but maybe in this case DeleteItems might be a better name. Choose clarity over entertainment value.
### 14. Pick One Word per Concept
- Pick one word for one abstract concept and stick with it. For instance, it’s confusing to have fetch , retrieve, and get as equivalent methods of different classes.
### 15. Dont use same name to mean 2 different things.
- exp: payInfo to represent amount to pay and payInfo to represent who to pay and bank info, best employeePaymentAmount and employeeBankDetails
### 16. Use Domain Specific Names
- exp : accountCotroller (indicating a controller), jobQueue(indicating queue)
### 17. Dont Make to Long or to Short Name
- to short means difficult to understand, to long means difficult to pronounce







