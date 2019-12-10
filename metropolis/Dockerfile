FROM metropolis-base:latest

USER root
RUN dnf install -y texlive-scheme-full

USER jovyan
RUN pip3 install --user nteract_on_jupyter \
    && jupyter serverextension enable --py nteract_on_jupyter
RUN pip3 install --user jupyterlab
RUN pip3 install --user jupyterlab-git \
    && jupyter labextension install @jupyterlab/git \
    && jupyter serverextension enable --py jupyterlab_git
RUN pip3 install --user jupyterlab-latex \
    && jupyter labextension install @jupyterlab/latex \
    && jupyter serverextension enable --py jupyterlab_latex
