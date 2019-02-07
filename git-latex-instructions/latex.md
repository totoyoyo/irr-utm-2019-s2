# Native LaTeX

LaTeX is free and open source software, which means that you can run it on pretty much any computer you want, given the technical know-how. That frees you from reliance on an internet connection to use ShareLaTeX. Fortunately, for the most popular operating systems, it is now relatively easy to get up and running with LaTeX. Below, you can find instructions for Mac (coming soon), Windows and Linux.

## Mac

### Installing LaTeX

You should install the full version of [MacTeX](https://tug.org/mactex/). This contains all of TeXLive plus some extra enhancements for Mac.

### Editing LaTeX files

You can use whatever text editor you prefer to compose and edit LaTeX files.

### Compiling LaTeX files

To compile your LaTeX file to pdf, you can open a bash terminal and use the script you find in the Linux instructions below.

## Windows 7-10

### Installing LaTeX

There are a few ways to get LaTeX for Windows, but I recommend using [MikTeX](https://miktex.org/download), as it is relatively simple to install and keep updated.

Depending on your needs, you may choose a full installation or a basic installation. If you just get a basic installation, then you may need internet access later if you want to get access to some advanced functionality. Adding any extra "packages" required at a later date is fully automated (you likely won't even notice it happening!), unless you have no internet access at the time. If you are worried about storage space on your computer, then get the basic installer; if you have plenty of space select the Net Installer and select a full installation. You should select 64-bit or 32-bit as appropriate for your version of Windows. During the installation, be sure to tell MikTeX to install missing packages *without* asking.

### Editing LaTeX files

You can use whatever text editor you prefer to compose and edit LaTeX files. Whetever editor you use, it is helpful to set it up a little to edit LaTeX files more conveniently.

I recommend using [notepad++](https://notepad-plus-plus.org/download/) as it is powerful, but has a reasonably friendly user interface. Download the latest version for Windows. You should select 64-bit or 32-bit as appropriate for your version of Windows.

Once you have notepad++ installed, you should install the plugin NppExec. You can find this in the menu "Plugins\Plugin Manager\Show Plugin Manager".

### Compiling LaTeX files

Although a LaTeX source file is just a plain text file, the output is a beautiful pdf of typeset mathematics, which can be read with any pdf viewer. The process of producing that output is handled by the program `pdflatex` and various other programs it calls. All of these programs are part of MikTeX.

In principle, you can just run
```
pdflatex.exe myfile.tex
```
from the command line and get a typeset document output as `myfile.pdf`, but there are some complications. Because of the way LaTeX works, in particular references to equations/theorems, it can take a few runs before the document is perfectly typeset. Outside of artificial examples or very long books, it is sufficient to run LaTeX 3 times.

You can do this conveniently using a batch file, called using the notepad++ plugin "NppExec". This takes a bit of set-up, but is quick to do once you have completed the set-up. First use the menu "Plugins\NppExec\Execute..." and enter

```
NPP_SAVE
C:\Program Files (x86)\Notepad++\pdflatex_build.bat "$(FULL_CURRENT_PATH)" $(CURRENT_LINE)
```
Click "Save..." and enter the name `latex_build`. Now go to menu "Plugins\NppExec\Advanced Options..." and add a menu item with name `pdflatex_build` and associated script "latex_build". Next go to menu "Settings\Shortcut mapper...". On the "Plugin commands" tab, scroll down until you find `pdflatex_build` and give it the shortcut `Ctrl+Shift+B`. All the above means that when you press `Ctrl+Shift+B`, notepad++ will save your current file and then run the batch file "pdflatex_build.bat". Now we just need to create that batch file!

To make the batch file script, open a new tab in notepad++ and paste the following into the file.

```
@echo off
if "%~x1"==".tex" goto pdflatex
   echo "%~f1" is not a recognized type, extension is "%~x1"
   pause
   exit /b

:pdflatex
   cd /d "%~dp1"
   IF NOT EXIST "%~dp1\build" mkdir build
   IF NOT EXIST "%~dp1\build" mkdir pdf

   start "inverseSearch" /min "%PROGRAMFILES(x86)%\SumatraPDF\SumatraPDF.exe" -inverse-search "\"%PROGRAMFILES(x86)%\Notepad++\notepad++.exe\" -n%%l \"%%f\"" -reuse-instance

REM   pdflatex.exe -draftmode -interaction=batchmode -aux-directory="%~pd1\build" -output-directory="%~pd1\build" "%~pdn1"
   pdflatex.exe --interaction=batchmode --aux-directory="%~pd1\build" --output-directory="%~pd1\pdf" "%~pdn1"
   echo. && echo.
   bibtex.exe "%~dp1\build\%~n1.aux"
   echo. && echo.
   pdflatex.exe --draftmode --interaction=batchmode --aux-directory="%~pd1\build" --output-directory="%~pd1\pdf" "%~pdn1"
   echo. && echo.
   pdflatex.exe --interaction=batchmode --synctex=-1 -aux-directory="%~pd1\build" --output-directory="%~pd1\pdf" "%~pdn1"
   echo. && echo.
   
   type "%~dp1\build\%~n1.log" | findstr Warning:
   
   start "openPDF" "%PROGRAMFILES(x86)%\SumatraPDF\SumatraPDF.exe"  "%~dp1\pdf\%~n1.pdf" -reuse-instance

   echo SUMATRA>"%~dp1\build\cmcdde.tmp"
   echo control>>"%~dp1\build\cmcdde.tmp"
   echo [ForwardSearch("%~dp1\pdf\%~n1.pdf", "%~f1", %2, 0, 0, 0)]>>"%~dp1\build\cmcdde.tmp"

   "%PROGRAMFILES(x86)%\cmcdde.exe" @"%~dp1\build\cmcdde.tmp"

   del "%~dp1\build\cmcdde.tmp"
   exit /b
```

Once we have finished, this batch file will be able to call LaTeX, call BibTex (usefull if you want to make bibliographies in LaTeX documents), then call LaTeX another two times. It also neatly arranges the output files so that the pdf file appears in a `pdf` subdirectory, and all the temporary & log files used to generate the pdf are placed in a separate `build` subdirectory. When you have lots of tex files, it helps to have the source tex files separate from all the other files!

To make the batch file work, you need to save it somewhere. I like to put it in the notepad++ directory, which for me is at
```
C:\Program Files (x86)\Notepad++\pdflatex_build.bat
```
Note that you will probably have to save the file somewhere else first, then copy-paste it into that directory.

### Viewing PDF files produced by LaTeX

There are plenty of programs that can be used to view pdf files, including the ubiquitous Adobe Reader, which is probably already installed on your computer. However, there are programs that are much faster than Adobe Reader. I recommend the 32-bit version of [SumatraPDF](https://www.sumatrapdfreader.org/download-free-pdf-viewer.html). As well as running much faster than Adobe Reader, SumatraPDF also allows you to use SyncTeX. SyncTeX means that you can double-click anywhere on your pdf, and notepad++ will move the focus to the corresponding part of the source file. This is very useful for editing long documents!

Now you are set up to work with LaTeX on Windows. Whenever you want to view your LaTeX output, just press `Ctrl+Shift+B` while editing the source file in notepad++.

## Linux Debian (Stretch) Gnome

### Installing LaTeX

LaTeX is distributed as part of the TeXLive family of packages. It is possible to install just certain parts of TeXLive, but it is usually good to get the whole thing, as adding extra packages later is not entirely straightforward, and will be impossible without an internet connection.

To install the full TeXLive package (~6GB) open up a terminal and run as `su`
```bash
apt-get install texlive-full
```
This will take some time to download.
If you prefer a smaller installation, you can install `texlive-base` instead.

### Editing LaTeX files

You can use whatever text editor you prefer to compose and edit LaTeX files. Whetever editor you use, it is helpful to set it up a little to edit LaTeX files more conveniently.

In Gnome, the default editor is GEdit (Text Editor), so here are some suggested settings. Under "Preferences/Plugins", enable the plugins "External Tools" and "SyncTeX". Under "Preferences/View", enable "Display line numbers", "Display statusbar", "Enable text wrapping", "Do not split words over two lines", "Highlight current line" and "Highlight matching brackets". Under "Preferences/Editor" enable "Enable automatic indentation".

### Compiling LaTeX files

Although a LaTeX source file is just a plain text file, the output is a beautiful pdf of typeset mathematics, which can be read with any pdf viewer. The process of producing that output is handled by the program `pdflatex` and various other programs it calls. All of these programs are part of TeXLive.

In principle, you can just run
```bash
pdflatex myfile.tex
```
from the terminal and get a typeset document output as `myfile.pdf`, but there are some complications. Because of the way LaTeX works, in particular references to equations/theorems, it can take a few runs before the document is typeset perfectly typeset. Outside of artificial examples or very long books, it is sufficient to run LaTeX 3 times.

You can do this conveniently using a bash script, called using the GEdit plugin "External Tools". Under "Manage External Tools..." add the new script
```bash
#!/bin/sh
# call my pdflatex_build shell script on the current document.

pdflatex_build.sh "$GEDIT_CURRENT_DOCUMENT_PATH"
```
with Shortcut key `Shift+Ctrl+B`, Save `All documents` and Applicability `All documents`, `LaTeX`.

The above means that whenever you press `Shift+Ctrl+B` the shell script `pdflatex_build.sh` will be called with a single argument: the document currently open in GEdit. Of course this will not work until we write the shell script `pdflatex_build.sh` and put it somewhere the system can find it!

To make the shell script, open a new tab in GEdit and paste the following into the file.

```bash
#!/bin/bash
# complete compile in pdflatex, including bibtex
# input $1 is tex file to be built, including path and extension
# synctex currently not implemented


# variables for file names, paths, etc.
TexPathFileExt=$1
TexPath=$(dirname "$TexPathFileExt")
AuxDir="${TexPath}/build/"
OutDir="${TexPath}/pdf/"
TexFileExt=$(basename "$TexPathFileExt")
TexFile="${TexFileExt%.*}"
AuxPathFileExt="${AuxDir}/${TexFile}.aux"
OutPathFileExt="${AuxDir}/${TexFile}.pdf"

# check that the source tex file exists, and exit if it does not
if [[ ! -f ${TexPathFileExt} ]] ; then
    echo "File '"${TexPathFileExt}"' is not there, aborting."
    echo "Please pass a full file path, including extension, as the only input parameter."
    exit
fi

# make build and output directories if they don't already exist
mkdir -p ${AuxDir}
mkdir -p ${OutDir}

# run pdflatex
pdflatex --draftmode --interaction=batchmode --output-directory="${AuxDir}" "${TexPathFileExt}"

# run bibtex
bibtex "${AuxPathFileExt}"

# run pdflatex twice more
pdflatex --draftmode --interaction=batchmode --output-directory="${AuxDir}" "${TexPathFileExt}"
pdflatex --interaction=batchmode --output-directory="${AuxDir}" "${TexPathFileExt}"

# move pdf output file to output directory
if [[ ! -f ${OutPathFileExt} ]] ; then
    echo "No output file produced, aborting."
    echo "Please pass a full file path, including extension, as the only input parameter."
    exit
fi
mv "${OutPathFileExt}" "${OutDir}/${TexFile}.pdf"
```

Once we have finished, this script will be able to call LaTeX, call BibTex (useful if you want to make bibliographies in LaTeX documents), then call LaTeX another two times. It also neatly arranges the output files so that the pdf file appears in a `pdf` subdirectory, and all the temporary & log files used to generate the pdf are placed in a separate `build` subdirectory. When you have lots of tex files, it helps to have the source tex files separate from all the other files!

To make the shell script work, save it as `pdflatex_build.sh` in the `Documents` directory, then run the following in a terminal as `su`:

```bash
cp Documents/pdflatex_build.sh /usr/local/bin/pdflatex_build.sh
chmod +x /usr/local/bin/pdflatex_build.sh
```

Now you are set up to work with LaTeX on Linux Debian (Stretch) Gnome. Whenever you want to view your LaTeX output, just press `Shift+Ctrl+B` while editing the source file in GEdit.
