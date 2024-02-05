# Tracking changes with latexdiff (for article resubmissions)

We assume that:
* You work in [Overleaf]([your overleaf project])
* You have a LaTeX editor installed, e.g. [texshop](https://pages.uoregon.edu/koch/texshop/obtaining.html)
* You have [latexdiff](https://ctan.org/pkg/latexdiff?lang=en) (command line tool) installed

## Step by step

* Label the overleaf project at first submission: go to `[your overleaf project] > History > All history > [latest documented change] > More actions (3 dots) > Label this version > [choose your label]`
* After reviewing & introducing changes into the overleaf project, (recommended) do the labelling again for the re-submission 
* to produce the diff pdf, download both overleaf projects:
	* to download the current version: `[your overleaf project] > Download > Source`
	* to download the previous (submitted) version: `[your overleaf project] > History > Labels > [choose the label] > More actions (3 dots) > Download this version`
* compile both separately in your local latex editor (to get .bbl output files from bibtex). "Compile" = Run LaTeX, then BiBTex, then 2x LaTex compiler
* in terminal, navigate to **new** project folder
* run from command line:
`latexdiff --flatten [filepath to old tex file] [name of new tex file] > diffs.tex`
* this will produce the diffs.tex file in the current folder; to produce the diffs PDF, compile the diffs tex