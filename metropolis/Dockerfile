FROM docker.pkg.github.com/fgborges/jupyter-metropolis/metropolis-base:latest
USER root
RUN dnf install -y texlive-scheme-full
USER jovyan
RUN pip3 install --user jupyterlab nteract_on_jupyter
RUN jupyter labextension install jupyterlab-drawio
RUN jupyter labextension install @jupyterlab/git \
    && pip3 install --user jupyterlab-git
RUN jupyter labextension install @jupyterlab/latex \
    && pip3 install --user jupyterlab_latex
