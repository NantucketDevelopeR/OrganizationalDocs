#Testing

##Topics
1. Writing simple tests for your functions
2. Using an integrated testing framework ([TestThat](https://github.com/hadley/testthat))

##Writing (good) Tests

+ Each test should test one behavior. 
+ One common type of test is the expectation. Some common examples of expectations can be found [here](http://r-pkgs.had.co.nz/tests.html). Expectations define behaviors or data properties that are expected to be true.
+ Tests can often be grouped into preconditions (things that must be true at the start of a function), postconditions (things that must be true at the end of a function, and invarients (which must be true at some point in the program's execution) 
+ Tests can have varying behaviors. 



##Using an integrated framework

We'll use testthat for this. Why?

+ Testthat is currently supported
+ Has informative failure messages
+ Widely Used

###Getting Started

+ Download testthat 
+ The test structure for testthat is a little different. You will have a standard 'test' folder, and a 'testthat' subfolder inside of it. 
+ Inside the test folder, you will place a file called 'testthat.R'
+ This file will contain the following lines:  


```
library(testthat)   
library(YourPackageName)	   
test_check("YourPackageName")	   
```

+ All the tests and accessory files will go inside the testthat folder. 
+ Test file names should be in the format test-NameOfTest.R. Accessory files or input can be named however.
+ To keep your output straight, it is best to put one set of tests in one file.
+ Now when you run  R CMD CHECK YourPackageName (or devtools::check() in RStudio), this will run all of the tests, as well.
+ While we're talking about R CMD CHECK, do note that you should add this to your DESCRIPTION file, under the 'suggestions' header.
+ You can use the devtools function add_travis to use Travis CI with this testing library to get those snazzy 'Build Passing' badges.
