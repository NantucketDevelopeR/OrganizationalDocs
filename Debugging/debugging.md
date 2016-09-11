#Bug fixing & debugging

## Bug fixing 
Brown & Murdoch (2016) give the following 5 steps to fix a bug:

1. Recognize that there is a bug.
2. Make the bug reproducible.
3. Identify the the cause of the error.
4. Fix the bug and test your new code.
5. search for similar errors 

## Debugging
To identify where a bug has happened the function [`traceback()`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/traceback.html) is your friend.

Most important familiarize yourself with the functions [`browser()`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/browser.html),[`debug()`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/debug.html) and [`debugonce()`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/debug.html) !

`browser()` sets a breakpoint in your code. You can inspect variables and see if they are valid etc.

You can then execute debugger commands:

* `n` - next; execute the next line of code. To see the context of a variable `n` use `print(n)`
* `s` - step into function call
* `c` - continue; let the function continue.
* `f` - finish the current function
* `Q` - quit the debugger



### References 
Braun WJ, Murdoch DJ (2016) A first course in statistical programming with R, 2nd edition, Cambridge University Press

Wickham H (2014) Advanced R, Chapman & Hall [url](http://adv-r.had.co.nz/) 
