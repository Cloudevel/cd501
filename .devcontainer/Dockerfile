# Python 3.12 sobre Debian 12 (bookworm), imagen de devcontainer fijada por versión.
# Dependabot (ecosistema docker) propone las actualizaciones.
FROM mcr.microsoft.com/devcontainers/python:3.15-bookworm

# Jupyter con los kernels de Python y de Bash, con versiones fijadas.
COPY requirements.txt /tmp/cd501-requirements.txt
RUN python -m pip install --no-cache-dir -r /tmp/cd501-requirements.txt \
    && python -m bash_kernel.install --sys-prefix \
    && rm /tmp/cd501-requirements.txt
