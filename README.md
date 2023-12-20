lab1
================
Перов Кирилл

Лабораторная работа №1

## Цель работы

Пройти 4 теста по R Programming через пакет swirl

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.0
4.  Swirl

## План

1.  Установить библиотеку swirl
2.  Запустить в консоли swirl::swirl()
3.  Пройти 4 теста

## Шаги

### Установка swirl

Пакет swirl можно установить в RStudio Desktop с помощью команды:

    install.packages("swirl")

После успешной установки в консоли пишем команду:

    swirl::swirl()

чтобы начать прохождение тестов.

### 1. Basic Building Blocks

1.1) In its simplest form, R can be used as an interactive calculator.
Type 5 + 7 and press Enter.

``` r
5 + 7
```

    [1] 12

1.2) To assign the result of 5 + 7 to a new variable called x, you type
x \<- 5 + 7. This can be read as ‘x gets 5 plus 7’. Give it a try now.
To view the contents of the variable x, just type x and press Enter. Try
it now.

``` r
x <- 5 + 7
x
```

    [1] 12

1.3) Now, store the result of x - 3 in a new variable called y. What is
the value of y? Type y to find out.

``` r
y <- x - 3
y
```

    [1] 9

1.4) The easiest way to create a vector is with the c() function, which
stands for ‘concatenate’ or ‘combine’. To create a vector containing the
numbers 1.1, 9, and 3.14, type c(1.1, 9, 3.14). Try it now and store the
result in a variable called z.

``` r
z <- c(1.1, 9, 3.14)
z
```

    [1] 1.10 9.00 3.14

1.5) Type ?c and press Enter. This will bring up the help file for the
c() function. Type z to view its contents. Notice that there are no
commas separating the values in the output.

``` r
?c
```

    запускаю httpd сервер помощи... готово

1.6) You can combine vectors to make a new vector. Create a new vector
that contains z, 555, then z again in that order. Don’t assign this
vector to a new variable, so that we can just see the result
immediately.

``` r
c(z, 555, z)
```

    [1]   1.10   9.00   3.14 555.00   1.10   9.00   3.14

1.7) Numeric vectors can be used in arithmetic expressions. Type the
following to see what happens: z \* 2 + 100.

``` r
z * 2 + 100
```

    [1] 102.20 118.00 106.28

1.8) Take the square root of z - 1 and assign it to a new variable
called my_sqrt.

``` r
my_sqrt <- sqrt(z - 1)
my_sqrt
```

    [1] 0.3162278 2.8284271 1.4628739

1.9) Now, create a new variable called my_div that gets the value of z
divided by my_sqrt.

``` r
my_div <- z / my_sqrt
my_div
```

    [1] 3.478505 3.181981 2.146460

1.10) To see another example of how this vector ‘recycling’ works, try
adding c(1, 2, 3, 4) and c(0, 10). Don’t worry about saving the result
in a new variable.

``` r
c(1, 2, 3, 4) + c(0, 10)
```

    [1]  1 12  3 14

1.11) Try c(1, 2, 3, 4) + c(0, 10, 100) for an example.

``` r
c(1, 2, 3, 4) + c(0, 10, 100)
```

    Warning in c(1, 2, 3, 4) + c(0, 10, 100): длина большего объекта не является
    произведением длины меньшего объекта

    [1]   1  12 103   4

1.12) In many programming environments, the up arrow will cycle through
previous commands. Try hitting the up arrow on your keyboard until you
get to this command (z \* 2 + 100), then change 100 to 1000 and hit
Enter. If the up arrow doesn’t work for you, just type the corrected
command.

``` r
z * 2 + 1000
```

    [1] 1002.20 1018.00 1006.28

### 2. Workspace and Files

2.1) Determine which directory your R session is using as its current
working directory using getwd().

``` r
getwd()
```

    [1] "E:/Quarto/lab1"

2.2) List all the objects in your local workspace using ls()

``` r
ls()
```

    [1] "my_div"  "my_sqrt" "x"       "y"       "z"      

2.3) Assign 9 to x using x \<- 9.

``` r
x <- 9
```

2.4) Now take a look at objects that are in your workspace using ls().

``` r
ls()
```

    [1] "my_div"  "my_sqrt" "x"       "y"       "z"      

2.5) List all the files in your working directory using list.files()

``` r
list.files()
```

    [1] "_quarto.yml"    "lab1.qmd"       "lab1.rmarkdown" "lab1.Rproj"    

2.6) or dir().

``` r
dir()
```

    [1] "_quarto.yml"    "lab1.qmd"       "lab1.rmarkdown" "lab1.Rproj"    

2.7) As we go through this lesson, you should be examining the help page
for each new function. Check out the help page for list.files with the
command ?list.files.

``` r
?list.files
```

2.8) Using the args() function on a function name is also a handy way to
see what arguments a function can take.

``` r
args(list.files)
```

    function (path = ".", pattern = NULL, all.files = FALSE, full.names = FALSE, 
        recursive = FALSE, ignore.case = FALSE, include.dirs = FALSE, 
        no.. = FALSE) 
    NULL

2.9) Assign the value of the current working directory to a variable
called “old.dir”.

``` r
old.dir <- getwd()
```

2.10) Use dir.create() to create a directory in the current working
directory called “testdir”.

``` r
dir.create("testdir")
```

2.11) Set your working directory to “testdir” with the setwd() command.

``` r
setwd("testdir")
```

2.12) Create a file in your working directory called “mytest.R” using
the file.create() function.

``` r
file.create("mytest.R")
```

    [1] TRUE

2.13) This should be the only file in this newly created directory.
Let’s check this by listing all the files in the current directory.

``` r
list.files()
```

    [1] "_quarto.yml"    "lab1.qmd"       "lab1.rmarkdown" "lab1.Rproj"    
    [5] "mytest.R"       "testdir"       

2.14) Check to see if “mytest.R” exists in the working directory using
the file.exists() function.

``` r
file.exists("mytest.R")
```

    [1] TRUE

2.15) Access information about the file “mytest.R” by using file.info().

``` r
file.info("mytest.R")
```

             size isdir mode               mtime               ctime
    mytest.R    0 FALSE  666 2023-12-20 17:01:23 2023-12-20 17:01:23
                           atime exe
    mytest.R 2023-12-20 17:01:23  no

2.16) Change the name of the file “mytest.R” to “mytest2.R” by using
file.rename().

``` r
file.rename("mytest.R", "mytest2.R")
```

    [1] TRUE

2.17) Make a copy of “mytest2.R” called “mytest3.R” using file.copy().

``` r
file.copy("mytest2.R", "mytest3.R")
```

    [1] TRUE

2.18) Provide the relative path to the file “mytest3.R” by using
file.path().

``` r
file.path("mytest3.R")
```

    [1] "mytest3.R"

2.19) You can use file.path to construct file and directory paths that
are independent of the operating system your R code is running on. Pass
‘folder1’ and ‘folder2’ as arguments to file.path to make a
platform-independent pathname.

``` r
file.path("folder1", "folder2")
```

    [1] "folder1/folder2"

2.20) Create a directory in the current working directory called
“testdir2” and a subdirectory for it called “testdir3”, all in one
command by using dir.create() and file.path().

``` r
dir.create(file.path('testdir2', 'testdir3'), recursive = TRUE)
```

2.21) Go back to your original working directory using setwd(). (Recall
that we created the variable old.dir with the full path for the orginal
working directory at the start of these questions.)

``` r
setwd(old.dir)
```

### 3. Sequences of Numbers

3.1) The simplest way to create a sequence of numbers in R is by using
the `:` operator. Type 1:20 to see how it works.

``` r
1:20
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

3.2) That gave us every integer between (and including) 1 and 20. We
could also use it to create a sequence of real numbers. For example, try
pi:10.

``` r
pi:10
```

    [1] 3.141593 4.141593 5.141593 6.141593 7.141593 8.141593 9.141593

3.3) What happens if we do 15:1? Give it a try to find out.

``` r
15:1
```

     [1] 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1

3.4) Remember that if you have questions about a particular R function,
you can access its documentation with a question mark followed by the
function name: ?function_name_here. However, in the case of an operator
like the colon used above, you must enclose the symbol in backticks like
this: ?`:`. (NOTE: The backtick (\`) key is generally located in the top
left corner of a keyboard, above the Tab key. If you don’t have a
backtick key, you can use regular quotes.)

``` r
?':'
```

3.5) The most basic use of seq() does exactly the same thing as the `:`
operator. Try seq(1, 20) to see this.

``` r
seq(1, 20)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

3.6) This gives us the same output as 1:20. However, let’s say that
instead we want a vector of numbers ranging from 0 to 10, incremented by
0.5. seq(0, 10, by=0.5) does just that. Try it out.

``` r
seq(0, 10, by=0.5)
```

     [1]  0.0  0.5  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0  6.5  7.0
    [16]  7.5  8.0  8.5  9.0  9.5 10.0

3.7) Or maybe we don’t care what the increment is and we just want a
sequence of 30 numbers between 5 and 10. seq(5, 10, length=30) does the
trick. Give it a shot now and store the result in a new variable called
my_seq.

``` r
my_seq <- seq(5, 10, length=30)
```

3.8) To confirm that my_seq has length 30, we can use the length()
function. Try it now.

``` r
length(my_seq)
```

    [1] 30

3.9) There are several ways we could do this. One possibility is to
combine the `:` operator and the length() function like this:
1:length(my_seq). Give that a try.

``` r
1:length(my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

3.10) Another option is to use seq(along.with = my_seq). Give that a
try.

``` r
seq(along.with = my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

3.11) However, as is the case with many common tasks, R has a separate
built-in function for this purpose called seq_along(). Type
seq_along(my_seq) to see it in action.

``` r
seq_along(my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

3.12) If we’re interested in creating a vector that contains 40 zeros,
we can use rep(0, times = 40). Try it out.

``` r
rep(0, times = 40)
```

     [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
    [39] 0 0

3.13) If instead we want our vector to contain 10 repetitions of the
vector (0, 1, 2), we can do rep(c(0, 1, 2), times = 10). Go ahead.

``` r
rep(c(0, 1, 2), times = 10)
```

     [1] 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2

3.14) Finally, let’s say that rather than repeating the vector (0, 1, 2)
over and over again, we want our vector to contain 10 zeros, then 10
ones, then 10 twos. We can do this with the `each` argument. Try
rep(c(0, 1, 2), each = 10).

``` r
rep(c(0, 1, 2), each = 10)
```

     [1] 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2

### 4. Vectors

4.1) First, create a numeric vector num_vect that contains the values
0.5, 55, -10, and 6.

``` r
num_vect <- c(0.5, 55, -10, 6)
```

4.2) Now, create a variable called tf that gets the result of num_vect
\< 1, which is read as ‘num_vect is less than 1’.

``` r
tf <- num_vect < 1
tf
```

    [1]  TRUE FALSE  TRUE FALSE

4.3) Let’s try another. Type num_vect \>= 6 without assigning the result
to a new variable.

``` r
num_vect >= 6
```

    [1] FALSE  TRUE FALSE  TRUE

4.4) Create a character vector that contains the following words: “My”,
“name”, “is”. Remember to enclose each word in its own set of double
quotes, so that R knows they are character strings. Store the vector in
a variable called my_char.

``` r
my_char <- c("My", "name", "is")
my_char
```

    [1] "My"   "name" "is"  

4.5) Type paste(my_char, collapse = ” “) now. Make sure there’s a space
between the double quotes in the `collapse` argument. You’ll see why in
a second.

``` r
paste(my_char, collapse = " ")
```

    [1] "My name is"

4.6) To add (or ‘concatenate’) your name to the end of my_char, use the
c() function like this: c(my_char, “your_name_here”). Place your name in
double quotes where I’ve put “your_name_here”. Try it now, storing the
result in a new variable called my_name.

``` r
my_name <- c(my_char, "Georgy")
my_name
```

    [1] "My"     "name"   "is"     "Georgy"

4.7) Now, use the paste() function once more to join the words in
my_name together into a single character string. Don’t forget to say
collapse = ” “!

``` r
paste(my_name, collapse = " ")
```

    [1] "My name is Georgy"

4.8) In the simplest case, we can join two character vectors that are
each of length 1 (i.e. join two words). Try paste(“Hello”,“world!”, sep
= ” “), where the `sep` argument tells R that we want to separate the
joined elements with a single space.

``` r
paste("Hello", "world!", sep = " ")
```

    [1] "Hello world!"

4.9) For a slightly more complicated example, we can join two vectors,
each of length 3. Use paste() to join the integer vector 1:3 with the
character vector c(“X”, “Y”, “Z”). This time, use sep = “” to leave no
space between the joined elements.

``` r
paste(1:3, c("X", "Y", "Z"), sep = "")
```

    [1] "1X" "2Y" "3Z"

4.10) Vector recycling! Try paste(LETTERS, 1:4, sep = “-”), where
LETTERS is a predefined variable in R containing a character vector of
all 26 letters in the English alphabet.

``` r
paste(LETTERS, 1:4, sep = "-")
```

     [1] "A-1" "B-2" "C-3" "D-4" "E-1" "F-2" "G-3" "H-4" "I-1" "J-2" "K-3" "L-4"
    [13] "M-1" "N-2" "O-3" "P-4" "Q-1" "R-2" "S-3" "T-4" "U-1" "V-2" "W-3" "X-4"
    [25] "Y-1" "Z-2"

## Оценка результата

Было пройдено 4 теста по основам языка R.

## Вывод

В ходе данной работы, я получил базовые знания языка R и узнал основы
его синтаксиса.
