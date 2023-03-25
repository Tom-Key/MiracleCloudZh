Miracle Cloud's interactive analytics environment integrates with Jupyter Notebook. Jupyter Notebook is a shareable, replicable analysis.
Jupyter Notebook is an open source analytics environment where you could get real-time insight into research data through interactive analysis and visualizations. You can import data - including genomics or transcriptomic data stored in the cloud - and analyze it using custom or pre-built libraries in R or Python.<br>
The Jupyter Notebooks provide environment for newbies and it is portable and reproducible. Notebooks combine analytical methods and visualization in one place in an easy-to-understand and shareable way. As a logical evolution of traditional scientific papers, Jupyter Notebooks dramatically shorten the path between reading how analysis is done and actually reproducing it. It's hard to overstate how powerful this concept is and the impact notebooks have on the reusability and reproducibility in computational science.

<br>

#### Create a new Notebook

1. Click New Notebook, enter a name and select a language. Currently supported languages are Python and R.
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_36ce1449617735a58bc4a689c1d5781f.png)

2. Click the edit button on the right to edit the notebook.
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f47aaf7ab70e812ed97ff527e1be85ba.png)
<br>

3. There are three ways to run code in a cell:
	1. Select the cell and press Shift + Enter on your keyboard (your keyboard may show "return" instead of "enter").
		
	2. Click the Run icon in the menu bar.
		
	3. Use the appropriate command from the Cell drop-down menu.
		

Cells are part of the Notebook. Each cell has a "type" (Code/Markdown/Raw NBConvert/Heading) that determines how the application computation will interpret the instructions in the cell.

<br>

#### How does code cell work?

When you run a code cell, the Jupyter compute kernel reads the code, passes those code to the actual operating system running Jupyter (eg Python, R), and retrieves the results to display them in a notebook. When the command runs, the output log of the command appears directly below the code cell.
If the code in the cell does not match the language of the kernel, the application calculation will return an error. If no output is specified, you will know that the code executed successfully by paying attention to the number in square brackets \[\] on the left side of the cell.
<br>

#### How to know if cell is executing?

When starting the notebook for the first time, the square brackets to the left of each cell are empty \[\], indicating that those units have not been run during this session. An asterisk in parentheses \[\*\] indicates that the unit is running. Once a command is executed, the asterisk is replaced by an integer representing the number of commands executed since the kernel started. You can execute the same cell multiple times, or execute multiple commands in one cell.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_46099de6daf4afbe809340ef6e816f23.png)
If you clear the output by going to the drop-down menu **Cells** > **All Outputs** and selecting **Clear**, the integer brackets will again be replaced by empty brackets. However, if you restart the kernel, the integer count will only reset to zero.
<br>

#### How to edit content in Markdown cells

Notebook can not only run python or R language code, but also supports editing content in Markdown format. First, please switch the cell type to Markdown, as shown in the figure below. Then please double-click the cell you want to edit.

Markdown is a lightweight plain text formatting language. It replaces typesetting with concise syntax, unlike the general word processing software Word or Pages that we use with a lot of typesetting and font settings. Here are some syntax examples:
*In* *Markdown**, if a piece of text is defined as a title, just prefix the text with a "#" .* 
Example: # first-level title
*If you need to quote a short sentence from elsewhere, use the quoted format. Just add angle brackets (>) before the text.* 
*The syntax of inserting a link is very similar to inserting a picture**, the* *difference is a "! "sign*
Image example：!\[\](){ImgCap}{/ImgCap}
Link example：\[\]()
