python2-with-latex
==================

Repository:
https://github.com/t3docs/docker-python2-with-latex

Docker store:
https://store.docker.com/community/images/t3docs/python2-with-latex

Description
-----------

This image is based on python:2 which in turn builds on Debian.
It has the rather large LaTeX packages installed (texlive-recommended)
and includes the Share font for LaTeX. The share font is often 
used in TYPO3 projects. The image is rather big but should not 
change very often.

Use this image as a drop in replacement for the regular `python:2` image
if you desire LaTeX functionality like PDF generation with `pdflatex`.

It is planned to add documentation at
https://docs.typo3.org/typo3cms/RenderTYPO3DocumentationGuide/UsingDocker/


Example usage
-------------

Go to a folder `latex` with the files of your TeX project.
Run the image for a single command. Remove the container
afterwards (`--rm`) automatically. Do a volume mapping for
the current folder and have it appear as `/data` inside 
the container. Run as current user 
(`--user=$(id -u):$(id -g)`). Append a single command like 
`make LATEXMKOPTS="-silent"`.

Run a single command:

    # go to some texfiles
    cd latex
    docker run --rm -v "$PWD":/data \
        --user=$(id -u):$(id -g) \
        t3docs/python2-with-latex \
        make LATEXMKOPTS="-silent"
        
To start interactive mode add the `-it` switches
and start the shell (`/bin/bash`):

    # go to texfiles
    cd latex
    docker run --rm -v "$PWD":/data \
        -it \
        --user=$(id -u):$(id -g) \
        t3docs/python2-with-latex \
        /bin/bash
        
Building
--------

Regular build with quite of bit of downloading:
        
    docker build -t t3docs/python2-with-latex .
    
In case you have an [apt-cacher](https://docs.docker.com/engine/examples/apt-cacher-ng/) at hand this may be
the way. Apt-packages are then downloaded only once and kept
for later use:
    
    docker start my-apt-cacher
    HOSTIP=$(ip route get 8.8.8.8 | awk '{print $NF; exit}')
    docker build -t t3docs/python2-with-latex --build-arg http_proxy=http://${HOSTIP}:3142 .
  
        
Finally
-------

Enjoy!
