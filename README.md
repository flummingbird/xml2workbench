# xml2workbench
Transform XML documents to a CSV for Islandora Workbench.
# to use xml2workbench with LDL data:
git checkout ldl-data
### for now we are supressing some data from the mods xml files. compare the original code to this branch here: https://github.com/flummingbird/xml2workbench/compare/main...flummingbird:xml2workbench:ldl-data
### NOTE: YOU will be required to edit the code to reflect the filepaths you're creating on your own computer.



git clone xml2workbench from a link orr

cd to directory of xml2workbench

determine your branch and origin before proceeding

git status 

(You should be using git not files copied from git without version control.)


Will write git steps

git clone 
git checkout ldl-data

nano xml2workbench.py

edit xml2workbench.py on line 560 and change the directory to reflect your own system.     

`directory = '/home/myUsername/Downloads/data-hnoc-aww/'` to `directory = '/home/yourUsername/Downloads/MODS_FILE_FOLDER/'`

python3 xml2workbench.py

Follow any error messages to their logical conclusion!
For now make notes of what lines give us problems and comment out code as necessary. We'll come back to this later.

Comment out code until you get an output.csv file that can be used in the next step
