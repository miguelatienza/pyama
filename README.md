## PyAMA
PyAMA is a desktop application for displaying TIFF stacks of single-cell microscopy images
and for reading out single-cell time courses of the cell area and the fluorescence intensity.

### Installation
Python 3.8 with `tkinter` is required.

Clone/pull/download this repository and make the base directory of the repository your working directory by using the `cd` command.
Then install the necessary dependencies either via anaconda or via pip, whatever you prefer.

#### Anaconda users
If you are using [Anaconda](https://www.anaconda.com), you can install the dependencies with the following command,
which will create a new environment called `pyama` and install all necessary packages:

```
conda env create -f environment.yml
conda activate pyama
```

#### Pip users
If you prefer using venv/pip, you can use this command
(if you use bash; otherwise, you may have to adapt the commands accordingly):

```
mkdir env
python -m venv env
source env/bin/activate
pip install -r requirements.txt
```

#### Desktop file installation for Linux users (experimental)
Linux users can create an application menu entry for PyAMA by adjusting the paths in the files `pyama.sh` and `pyama.desktop` and creating a symlink to (or copying) `pyama.desktop` in either `/usr/share/applications` (for global installation) or `~/.local/share/applications` (for user-specific installation).


### Usage
#### Starting PyAMA
Make sure that the environment containing PyAMA is activated.
Open the program with by executing the file `__main__.py` in the base directory of
this repository.
For simplicity, you can simply call the directory containing the file, and python
will find the file for you. For example (if you are in the parent directory of this repository):

```
python pyama
```

Alternatively, if your current working directory is the base directory of this repository,
you can execute

```
python __main__.py
```

or simply:

```
python .
```

Upon starting PyAMA, an empty window opens.

#### Loading a stack
To open a stack, click on “Open stack…” (or, alternatively: “File/Open stack…”).
A stack selection dialog is displayed.
By clicking on “Open”, you can load a TIFF stack.
On the right side of the dialog, you can compose your analysis stack from the
channels of multiple files.
To add a channel to the analysis stack, select the corresponding file on the left
side of the dialog, then specify the channel, type and optional label of the channel
on the right side and finally, click on “Add”.

The channel is the number of the channel in the selected file.
This requires that you know in which order the channels are arranged in your file.

The type of the channel is one of the following:

* “Phase contrast”: The channel is a phase contrast image. It is only displayed for your  
  orientation.
* “Fluorescence”: The channel contains fluorescence images. This channel will be used  
  for fluorescence readout.
* “Segmentation”: The channel contains a binary image indicating the cells as  
  non-zero areas. This channel will be used to detect and track the cells and  
  integrate the fluorescence over the cells.  

  Technical note: Each cluster of 1-connected (4-neighborhood) non-zero pixels is treated  
  as one cell. To be trackable, the contours of a cell must overlap in subsequent frames.  
  Contours below a threshold are ignored.

The optional label is only used for documenting the stack and has no effect on the
fluorescence readout. For example, you can label a fluorescence channel as GFP to
distinguish it from other fluorescence channels.

When all channels are specified, click on “OK” to load the stack.
Depending on the stacks you specified, this may take some time, but the loading progress
is shown in the status line.

#### Viewing the stack
You can review the stack by selecting the channel to display, by scrolling through
the frames using the slider below the stack, and by highlighting single cells.

You can highlight a cell by clicking on the cell in the image or on a corresponding
trace in the plot axes.

To exclude or include a cell in the readout, select/deselect it by clicking on the
cell in the image with the shift key pressed.
You can also (de)select highlighted cells with the enter key.

You can also use the up/down arrow keys to scroll through the cells,
the left/right arrow keys to scroll through the frames,
and the number keys to change the displayed channel.
Use Ctrl + left/right arrow to scroll 10 frames at once and Home/End to jump to the first/last frame.

When all cells to be read out are specified, click on “File/Save” to save the measurements.
Note that highlighted cells will also be highlighted in the plot in the PDF file.

When saving the measurements, a file `session.zip` is generated.
You can reload the measurement by clicking on “File/Load session” and selecting this file.
Currently, this requires that the stack files are all located in the same directories
as during the session that created the file.

#### Pyama on a Notebook

You can use Pyama on a Notebook to run the segmentation and background correction of all positions at once.
This will save your segmentation and background correction for each position in a separate folder named XY{position} wherever you specify that you would like to save your data.

To use this run the following from the main directory where pyama is located:
```
source env/bin/activate
```
If you are running pyama on a notebook for the first time:
```
python -m ipykernel install --user --name pyama
```

Then if you want to have access to directories outside pyama, cd into a different directory.
Finally open jupyter-lab:
```
jupyter-lab
```
In jupyterlab: open notebooks_tests/lisca.ipynb or copy the file to your working directory and follow the instructions to run segementation and background correction on your data.
