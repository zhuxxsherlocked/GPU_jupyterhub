# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.

FROM bsbii/scipy-notebook:1.0

# Install jupyterlab
RUN conda install -c conda-forge jupyterlab
RUN jupyter serverextension enable --py jupyterlab --sys-prefix

USER jovyan