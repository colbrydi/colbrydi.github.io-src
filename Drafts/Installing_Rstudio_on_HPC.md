# Installing RStudio on the MSU HPCC

These instructions are for installing RStudio on the HPCC. RStudio provides a variety of installers on their website (https://www.rstudio.com/products/rstudio/download2/). However, the HPCC uses an older version on Linux and the precompiled binaries are not compatible.  Trying to install RStudio from the source code is an option but is also difficult due to the large numbers of libraries and compile dependancies.  I basically gave up trying to install RStudio on until I realized that a version is included as an option in Anaconda.  The following instructions show how to install RStudio using Anaconda.

## **Step 0:** Connect to the HPC using ssh and make sure you have a working X11 server (ex. MobaXterm on windows or XQuarts on Mac).  More information about X11 can be found here:

<<<Link to HPCC X11 tutorial>>

## **Step 1:** Download the Linux Anaconda installers

Find the latest Linux installer on the Anaconda website.  Use the wget command to download the script file.

'''
wget ...
'''

## **Step 2:** Install anaconda using the defaults

'''
bash Anaconda...
'''

The last question will ask you to add the anaconda folder to your bashrc file. Say yes...

## **Step 3:** Use the conda install command to install RStudio

'''
conda install rstudio
'''

That should be it, RStudio is not installed in your home directory on the HPCC.  The only other thing you will need is a X11 server.  
