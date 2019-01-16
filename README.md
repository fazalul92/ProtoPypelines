# ProtoPypelines
### Currently in ideation phase, contributions are always welcome.
A web application to generate pipelines from Jupyter Notebooks for rapidly evolving scientific studies. This application is not intended to replace the use of Jupyter Notebook but rather to complement it. The application works as a standalone web app that can be downloaded and extracted to any computer. Once the initial setup is complete, "ipynb" files or directories containing them can be added to be parsed. 

## The Problem
In the realm of data science/machine learning research, there is rarely any cases where you run into the best solution in the first try. It is through continuously changing the input data and trying out different models that you eventually end up with an ideal solution. While trying to run experiments, I've often ended up creating multiple files with data processed in different ways, and wrote multiple scripts to try to build different models using these files. Within a short span, I mostly end up generating a folder full of csv files and scripts (with 1_1, 1_2, 2_1 etc. as filenames) and forget the outputs I've gotten for each of the scripts. With the granualar changes in parameters that are made, it becomes confusing to even document these changes and results in any file. If I were to even consider running a previously written script, the inputs, outputs, variables and parameters for the files have to be found out again, and I might even have overwritten on the input files sometimes. 

It is due to this that I've written up the requirements for an application that might be helpful to many researchers/ML engineers.


## Initial Requirements
#### Broader Requirements
* Generate workflows/pipelines from existing files without much modification.
* Minimal code for implementation
* Integration with existing technology(ies)
* GUI for better code interpretability
* Interoperability with Jupyter Notebooks
	* Opening and navigating to Jupyter cells from ProtoPypeline graphs
	* Running code on Jupyter/python without opening the specific files
* Avoiding installation of python modules for usage
	* Usage of special comment tags (eg. #?#) for specifying DOM elements/description
* Uses native resources for computation. Options to use the kernels specificed in Jupyter.


#### Working
1. Import an "ipynb" file to parse or in case of folders, import all .ipynb files from folder under a project name.
2. Read the cells to find root node(s) and add it to the DOM. 
3. Read cells to find "cell labels" (specified using special comment tages) and add them to the DOM/dictionary
4. Read cells to find "cell descriptions" and add them to the DOM/dictionary
5. Read cells to find "child cells" that have their parents specified using the "cell labels".
6. Find cells that take in input from the user and add a special child as an input textbox.
7. Find cells with outputs in the form of print (console), or files/graphs. 
	1. If the cell contains a print statement, store the contents being printed and add it as a special child for the cell
	2. If the cell contains a graph/plot, store them as images and add it's reference as a special child.
	3. If the cell contains a file output (like image, csv, etc), use the reference as a special child.
8. Generate a graph structure in a root-down manner. 
	1. The structure has the root at the top and every child comes below it.
	2. Each child to a parent is displayed horizontally, in the chronological order of cell creation.
	3. Cell descriptions (if available) and timestamp of cell creation should be shown when a mouse hover is detected.
	4. Special child cells are shown beside the parent cell. Such parent cells should have a signifier to show child presence.
	5. Special child cells with inputs required can have input boxes that can be populated.
	6. Special child cells with graphs, plots or images as references should be displayed.
	7. All cells that don't have a parent specified would be marked as a child of the preceeding cell.
	8. Display a clickable "Run" button at the last children on the graph. 
9. Running the code when a "run" button attached to a node is clicked:
	1. All the cells in the path from the root to the specific leaf/node should be run. No other pathways/cells should be run.
	2. If any cells require input:
		1. If the corresponding text cell is empty, request the user for input through a prompt.
		2. if the cell is not empty or the user has given his input, use the input and go to the next cell.
	3. Update the special children nodes and display the new output/results.
	4. A console displayed should also show all the outputs.
