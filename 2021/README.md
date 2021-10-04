# TeX Live 2021 - Docker image

There are three types of Docker images each using different TeXLive profiles:

1. thorwink/texlive:2021-minimal:
Based on TeX Live installation scheme "scheme-minimal" and contains only
essential programs and files

2. thorwink/texlive:2021-full
Based on TeX Live installation scheme "scheme-full" and contains all collections
and packages (except "collection-wintools" for Windows-only support programs)

3. thorwink/texlive:2021 (thorwink/texlive:latest)
This is my preferred image containing a custom installation for english and
german language. [(see 2. TeX Live profiles)](#texlive-profiles)

--- 
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [1. Usage](#1-usage)
- [2. TeX Live profiles](#2-tex-live-profiles)
- [3. Build & update Docker images](#3-build--update-docker-images)
- [4. Important hints/tipps](#4-important-hintstipps)
- [5. Tests](#5-tests)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 1. Usage

* The working directory inside the container is **/docs** and the variable
**$PWD** contains the path of the current directory.
```bash
    docker run --rm -v $PWD:/docs thorwink/texlive:latest pdflatex somedoc.tex
```

* pdflatex is only one of the programs you can use. Use the following command to
get the whole list of binaries available:
```bash
    $ docker run --rm thorwink/texlive:2021 ls -la /usr/local/texlive/2021/bin/x86_64-linux
```

* Work directly inside the container:
```bash
    docker run --rm -ti -v $PWD:/docs thorwink/texlive:latest bash
```

## 2. TeX Live profiles
These TeX Live profiles were used for the Docker images:

| #     | Collections                   | minimal   | custom    | full      |
|:-----:|-------------------------------|:---------:|:---------:|:---------:|
| 1 |**collection-basic**<br>Essential programs and files|x|x|x|
| 2 |**collection-bibtexextra**<br> BibTeX additional styles||x|x|
| 3 |**collection-binextra**<br>TeX auxiliary programs||x|x|
| 4 |**collection-context**<br>ConTeXt and packages||x|x|
| 5 |**collection-fontsextra**<br>Additional fonts||x|x|
| 6 |**collection-fontsrecommended**<br>Recommended fonts||x|x|
| 7 |**collection-fontutils**<br>Graphics and font utilities||x|x|
| 8 |**collection-formatsextra**<br>Additional formats||x|x|
| 9 |**collection-games**<br>Games typesetting||x|x|
|10 |**collection-humanities**<br>Humanities packages||x|x|
|11 |**collection-langarabic**<br>Arabic|||x|
|12 |**collection-langchinese**<br>Chinese|||x|
|13 |**collection-langcjk**<br>Chinese/Japanese/Korean (base)|||x|
|14 |**collection-langcyrillic**<br>Cyrillic|||x|
|15 |**collection-langczechslovak**<br>Czech/Slovak|||x|
|16 |**collection-langenglish**<br>US and UK English||x|x|
|17 |**collection-langeuropean**<br>Other European languages|||x|
|18 |**collection-langfrench**<br>French|||x|
|19 |**collection-langgerman**<br>German||x|x|
|20 |**collection-langgreek**<br>Greek|||x|
|21 |**collection-langitalian**<br>Italian|||x|
|22 |**collection-langjapanese**<br>Japanese|||x|
|23 |**collection-langkorean**<br>Korean|||x|
|24 |**collection-langother**<br>Other languages|||x|
|25 |**collection-langpolish**<br>Polish|||x|
|26 |**collection-langportuguese**<br>Portuguese|||x|
|27 |**collection-langspanish**<br>Spanish|||x|
|28 |**collection-latex**<br>LaTeX fundamental packages||x|x|
|29 |**collection-latexextra**<br>LaTeX additional packages||x|x|
|30 |**collection-latexrecommended**<br>LaTeX recommended packages||x|x|
|31 |**collection-luatex**<br>LuaTeX packages||x|x|
|32 |**collection-mathscience**<br>Mathematics,<br>natural sciences,<br>computer science packages||x|x|
|33 |**collection-metapost**<br>MetaPost and Metafont packages||x|x|
|34 |**collection-music**<br>Music packages||x|x|
|35 |**collection-pictures**<br>Plain (La)TeX packages||x|x|
|36 |**collection-plaingeneric**<br>Plain (La)TeX packages||x|x|
|37 |**collection-pstricks**<br>PSTricks||x|x|
|38 |**collection-publishers**<br>Publisher styles, theses, etc.||x|x|
|39 |**collection-texworks**<br>TeXworks editor; TL<br>includes only the Windows binary||x|x|
|40 |**collection-wintools**<br>Windows-only support programs||||
|41 |**collection-xetex**<br>XeTeX and packages||x|x|

## 3. Build & update Docker images
```bash
    # Clone git repository
    $ git clone https://github.com/thorsten-winkler/docker-texlive.git
    $ cd docker-texlive/2021

    # Build the first image (2021-minimal)
    $ docker build -f Dockerfile.minimal -t thorwink/texlive:2021-minimal .

    # My custom build refers to that 2021-minimal image
    $ docker build -f Dockerfile.custom -t thorwink/texlive:2021 .

    # The full build refers to the custom build 2021 image
    $ docker build -f Dockerfile.full -t thorwink/texlive:2021-full .

    ####################
    # Update existing Docker images using tlmgr
    $ docker run --rm thorwink/texlive:2021-minimal tlmgr install <package>
    $ docker run --rm thorwink/texlive:2021-minimal tlmgr install collection <collection>

```
## 4. Important hints/tipps
If you build these Docker images by your own, only keep in mind that
**thorwink/texlive:2021-full** is based on my custom image
**thorwink/texlive:2021** (thorwink/texlive:latest) which again is based on
**thorwink/texlive:2021-minimal**.
Background:
This procedure minimizes the setup time and therefore saves resources of
Dockerhub (using its Autobuild functionality).

## 5. Tests
Small sniff tests:
```bash
    $ docker run --rm thorwink/texlive:latest latex small2e
    $ docker run --rm thorwink/texlive:latest pdflatex
```
