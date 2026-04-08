# Submitting to arXiv

This file documents the steps for submitting a manuscript to arxiv. In this process, all your tex and bib sources will be made public, so you want to delete all comments, unused references, etc. Fortunately, this is automatized by two command line tools, see below.

We assume that:  
* You work in `Overleaf` (optional)  
* You have a `main.tex` with a `references.bib`, and possibly an `si.tex` possibly with its separate `si_references.bib`  
* You have [`bib_optimizer`](https://github.com/kwyip/bib_optimizer) and [`arxiv_latex_cleaner`](https://github.com/google-research/arxiv-latex-cleaner) installed.  
* You work on a mac.

## Step by step

* Label the Overleaf project just before the arxiv submission, for example "arxiv submission": go to `[your overleaf project] > History > All history > [latest documented change] > More actions (3 dots) > Label this version > [choose your label]`
* Download and unzip the project
* In the downloaded project folder, run: `bibopt main.tex references.bib references_cleaned.bib`
* Delete `references.bib`
* If you have an SI that uses the same `references.bib` and has additional entries that are not used in `main.tex`, manually copy all those entries into `references_cleaned.bib`. (If your SI has its own separate bib file `si_references.bib` though, you need to re-run all steps for `si_references.bib` that you run for `references.bib`)
* run: `sed -i '' 's/\bibliography{references}/\bibliography{references_cleaned}/' main.tex`
* if you have an SI, run: `sed -i '' 's/\bibliography{references}/\bibliography{references_cleaned}/' si.tex`  
* run: `arxiv_latex_cleaner /folder --compress_pdf --keep_bib`, where folder is your folder. This will create a new folder `folder_arXiv`
* On arXiv, [start a new submission](https://arxiv.org/user/create?preflight=1) and select the arXiv.org perpetual, non-exclusive license (or change this if you know what you are doing)
* Zip the contents of `folder_arXiv` and upload the zip file to arxiv
* If you have an SI, add the `si.tex` as a second "Top-Level TeX" in the "Review Files" section
* Metadata Comments: `Main text: X pages, X figures. SI: X pages, X figures`
* Continue the process and submit