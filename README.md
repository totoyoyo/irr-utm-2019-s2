# Independent reading & research: Unified transform method for linear evolution equations

Academic year 2018-2019 Semester 2

## Student lecture notes

You are each responsible for typing up your own lecture notes. You can build the file `GITREPO/master/student-lecture-NAME.tex` to typeset your lecture or `GITREPO/master/student-lecture-all.tex` to typeset all of your lectures. Each of these input the source files `GITREPO/student-lecture/NAME.tex`, which is where you should actually type up your lecture notes.

## Instructor lecture notes

Files for these will be added in future.

## Other files

The files `CourseHeadCenter.tex`, `CourseHeadLeft.tex` and `CourseHeadRight.tex` should not be edited. The file `texHead.tex` contains many macros definitions. You can also add your own macros if you like, so that they are always accessible.

## Being a good git citizen

Git should not be version controlling output pdf files build from tex, only the source tex files. This is because it is always easy to produce the pdf files (we all have access to pdflatex) given the source tex files, and it is a waste of everyone's time & bandwidth to be unnecessarily syncronising and storing file histories of the pdf binaries. You will find in `.gitignore` that the instructor has explicitly excluded from tracking the pdf files that are produced from each tex file. But also any directory `/build/` and `/pdf/` is explicitly ignored. If you follow the LaTeX set up instructions from the Proof module, then your pdf files will automatically be stored in these directories, hence ignored. Otherwise, if you create any new tex files you will have to manually add the corresponding pdf files to `.gitignore`.
