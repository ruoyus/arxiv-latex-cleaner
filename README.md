# `arxiv_latex_cleaner` Instruction

This tool allows you to easily clean the LaTeX code of your paper to submit to
arXiv. 

## Part 1  Basic Use


### Step 1: Clone (only first-time use)
   Clone or download this folder "arxiv-latex-cleaner" to your own computer.
   To clone, you can open the terminal, and type the following (note that /path should be changed to the folder where you want to put the folder "arxiv-latex-cleaner", such as /User/Yourname/Documents):
```console
cd /path
git clone https://github.com/ruoyus/arxiv-latex-cleaner.git
cd arxiv-latex-cleaner/
```
  Now you should be in the folder `/path/arxiv-latex-cleaner`
   
### Step 2: Test (only first-time use)
  To make sure the code works, we can run a simple test. There is a folder called `tex_test` which contains everything for a project (such as LaTex file, pdf file, bib file, images), and we want to create a new folder `/path/arxiv-latex-cleaner/tex_test_arXiv` that is ready to ZIP and upload to arxiv. 

#### Example call:

```console
python -m arxiv_latex_cleaner /path/arxiv-latex-cleaner/tex_test_arXiv
```
You should see a new folder `tex_test` created in the folder `/path/arxiv-latex-cleaner`

### Step 3: Generate your own arXiv
  Copy your own folder, say, `MyProject` into the folder arxiv-latex-cleaner. Then run
```console
python -m arxiv_latex_cleaner /path/arxiv-latex-cleaner/MyProject
```
  Should see a folder MyProject_arXiv. You can then open the Latex file in MyProject_arXiv to check whether the comments are removed, and whether you can generate the same pdf as before. 
  
### Understanding the folder
  In the folder `arxiv-latex-cleaner`, there is a sub-folder `arxiv_latex_cleaner` which contains the main code. The difference between the parent-folder and child-folder is `-` v.s. `_` between words.
  The test_tex file is set up to check a few things. You can check the folder to figure out. 
  
============  ============  ============  ============  ============  ============  ============  <br/>  
============  ============  ============  ============  ============  ============  ============  <br/>
## Part II  More Advanced
The following are not needed for the first-time use; may be useful for future use.  <br/>


### 2.1 Open Issues
 #### 2.1.1 Other Comments 
 I'm not sure how to modify the code to remove the contents between "\iffalse" to "\fi". Also not sure how to remove the contents between "\ifSomething to \fi". 

 #### 2.1.2 Processing Images
 The original example call is the following: 
 ```
python -m arxiv_latex_cleaner /path/arxiv-latex-cleaner/tex_test_arXiv  --im_size 500 --images_whitelist='{"images/im.png":2000}'
```
 There are extra options on the image sizes, which I ignore.
 
 Compared to the original code, I made one modification: comment "from PIL import IMAGE"in the file "arxiv_latex_cleaner/arxiv_latex_cleaner.py", line 22.
  If uncomment this command (then back to the original file), then I get the following error message: "ImportError: No module named PIL". This is due to the lack of package PIL.
 1) I tried to install Pillow, but still does not work.
 ```
 pip install Pillow
 ```
 Should see "Successfully installed Pillow-7.1.1"
 BTW: If you have not installed pip, type the following two commands: 
 ```
 curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
 sudo python3 get-pip.py
 pip --version
 ```
 2) One blog suggested uninstall Pillow and install it again, but still does not resolve the issue. 
 ```
 pip uninstall Pillow
 pip uninstall PIL
 pip install Pillow
 ```




### 2.2 Hard-to-understand Part of Original Doc

Starting from here, everything is copied from the original document. I did not understand their meanings yet. 

Optionally, this may be installed and used as a command-line program:

```console
python setup.py install
arxiv_latex_cleaner --help
```

### 2.3 Main features (from original doc)

#### Privacy-oriented

*   Removes all auxiliary files (`.aux`, `.log`, `.out`, etc.).
*   Removes all comments from your code (yes, those are visible on arXiv and you
    do not want them to be). These also include `\begin{comment}\end{comment}`
    environments.
*   Optionally removes user-defined commands entered with `commands_to_delete`
    (such as `\todo{}` that you at the end redefine as the empty string).

#### Size-oriented

There is a 10MB limit on arXiv submissions, so to make it fit:

*   Removes all unused `.tex` files (those that are not in the root and not
    included in any other `.tex` file).
*   Removes all unused images that take up space (those that are not actually
    included in any used `.tex` file).
*   Optionally resizes all images to `im_size` pixels, to reduce the size of the
    submission. You can whitelist some images to skip the global size using
    `images_whitelist`.
*   Optionally compresses `.pdf` files using ghostscript (Linux and Mac only).
    You can whitelist some PDFs to skip the global size using
    `images_whitelist`.

## Usage:

```
usage: arxiv_latex_cleaner@v0.1.0 [-h] [--resize_images] [--im_size IM_SIZE]
                                  [--compress_pdf]
                                  [--pdf_im_resolution PDF_IM_RESOLUTION]
                                  [--images_whitelist IMAGES_WHITELIST]
                                  [--commands_to_delete COMMANDS_TO_DELETE [COMMANDS_TO_DELETE ...]]
                                  input_folder

Clean the LaTeX code of your paper to submit to arXiv. Check the README for
more information on the use.

positional arguments:
  input_folder          Input folder containing the LaTeX code.

optional arguments:
  -h, --help            show this help message and exit
  --resize_images       Resize images.
  --im_size IM_SIZE     Size of the output images (in pixels, longest side).
                        Fine tune this to get as close to 10MB as possible.
  --compress_pdf        Compress PDF images using ghostscript (Linux and Mac
                        only).
  --pdf_im_resolution PDF_IM_RESOLUTION
                        Resolution (in dpi) to which the tool resamples the
                        PDF images.
  --images_whitelist IMAGES_WHITELIST
                        Images (and PDFs) that won't be resized to the default
                        resolution,but the one provided here. Value is pixel
                        for images, and dpi forPDFs, as in --im_size and
                        --pdf_im_resolution, respectively. Format is a
                        dictionary as: '{"path/to/im.jpg": 1000}'
  --commands_to_delete COMMANDS_TO_DELETE [COMMANDS_TO_DELETE ...]
                        LaTeX commands that will be deleted. Useful for e.g.
                        user-defined \todo commands.
```

## Note

This is not an officially supported Google product.
