# Required software 

Here is a list of software that not everyone may have installed, but we are going to use, and that thus should 
be installed in advance. We can save a lot of time and trouble if you install this beforehand, often you will 
many programs installed already anyway. Also I am not sure how fast the connection will be if we all start 
downloading the first day.  

## Software to be installed in advance on your computer
* Latest version of [R](https://cran.r-project.org/) (version 3.3.1)
* Git 
  + you will also need a [github](https://github.com/) account
* Decent text editor for R ([Rstudio](https://www.rstudio.com/), Emacs + ESS)
* LaTeX ([Miktex](http://miktex.org/) on windows, package texlive on linux)
* C/C++ compiler:
    + For windows, install [Rtools](https://cran.r-project.org/bin/windows/Rtools/)
    + For mac, install [XCode](https://developer.apple.com/xcode/)
    + For linux, everything should be up and running.
* R Packages:
    + Essential for R package development, and for installing non-CRAN (Github) packages: devtools, testthat, maggrittr.
      You can install packages using the install.packages command inside R:
    ```
        install.packages(c("devtools", "testthat", "magrittr")     
    ```    
    + Documentation (vignettes, practicals, tutorials): knitr
    + Plotting: ggplot2, rgl
    + phylogenetics: ape, phangorn, phytools, pegas, Rphylip, coalescentMCMC 
    and some packages are hosted at the Biocoductor repository:
    ```
        source("https://bioconductor.org/biocLite.R")
        biocLite(pkgs=c("Biostrings", "seqLogo", "ggtree"))
    ```    
    To automatically install all (most) phylogenetic packages on CRAN you can use the ctv package.
    This will take some time, so ensure you have a fast internet connection and your laptop is connected to power:
    ```
        install.packages("ctv")      
        library("ctv")      
        install.views("Phylogenetics")     
    ```
To check that you can install and compile programs you can try to install the latest phangorn version from github:
    ```
       devtools:::install_github("KlausVigo/phangorn")
    ```  


