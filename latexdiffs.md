# Tracking changes with latexdiff (for article resubmissions)

There are two ways to produce a diff PDF with latexdiff. Choose the one that works best for you.

---

## Option 1: Manual approach (download and compile locally)

This approach requires you to have a LaTeX editor installed locally (e.g., [TexShop](https://pages.uoregon.edu/koch/texshop/obtaining.html)) and [latexdiff](https://ctan.org/pkg/latexdiff?lang=en) installed on your machine.

We assume that you work in [Overleaf]([your overleaf project]).

### Step by step

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

---

## Option 2: Automatic approach (using `latexmkrc` in Overleaf)

This approach runs latexdiff automatically every time you compile in Overleaf. No local tool installation required.

We assume that you work in [Overleaf]([your overleaf project]).

Overleaf uses [LatexMk](https://docs.overleaf.com/managing-projects-and-files/the-latexmkrc-file) to compile your project. You can create a `latexmkrc` file to customize the compilation process, including running `latexdiff` before the actual PDF compilation.

### How it works

1. Create a file named `latexmkrc` in your Overleaf project
2. Add the following content to configure latexdiff:

```latexmkrc
# Compile a single file normally (i.e. response.tex - comment out if needed)
#$pdflatex = "pdflatex %O response.tex"

# Run latexdiff to create a diff, then compile it
$pdflatex = "latexdiff -t UNDERLINE --flatten main_v1.tex main.tex > main-d.tex; pdflatex %O main-d"
```

### Explanation

- The first line is commented out but shows how you could compile a different file normally (just replace `response.tex` with your desired filename).
- The second line is the relevant configuration:
  - `latexdiff -t UNDERLINE --flatten main_v1.tex main.tex > main-d.tex`: runs latexdiff to compare `main_v1.tex` (the frozen/old version) with `main.tex` (the new version) and outputs the diff to `main-d.tex`
  - `-t UNDERLINE` sets the diff style (underline for deletions/additions)
  - `pdflatex %O main-d.tex` — compiles the diff file. `%O` passes through any options/flags Overleaf might add.
  - The semicolon `;` chains the two commands together.

### Important notes

- **`main.tex`** should contain your updated version with all the changes.
- **`main_v1.tex`** should be the frozen version (the last submitted version). Do not modify this file.
- If your document uses `\include` or `\input` to load additional TeX files, you may need to flatten those into a single file for the comparison to work correctly, or ensure both versions include their respective `_v1.tex` or `.tex`, then latexdiff will take care of the flattening.
- After uploading `latexmkrc`, Overleaf will automatically run latexdiff on every recompile (the manual compilation options are shwdows by this). The resulting PDF will show all changes marked up.

