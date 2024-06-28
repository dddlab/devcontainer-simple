FROM quay.io/jupyter/datascience-notebook:r-4.3.3

USER root

ENV R_STUDIO_VERSION 2023.12.1-402

### RStudio system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        psmisc lsb-release libssl-dev \
        libclang-dev libpq5 libtiff-dev \
        && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/* /tmp/library-scripts

### RStudio server
ARG R_STUDIO_URL="https://download2.rstudio.org/server/jammy/amd64"
RUN wget -q "${R_STUDIO_URL}/rstudio-server-${R_STUDIO_VERSION}-amd64.deb" && \
    apt-get install -yq --no-install-recommends ./rstudio*.deb && \
    rm -f ./rstudio*.deb && \
    apt-get clean && \
    chmod 777 /var/run/rstudio-server && \
    chmod +t /var/run/rstudio-server

USER ${NB_USER}

### RStudio launch button in Jupyter Lab
RUN mamba install -y -c conda-forge --freeze-installed \
        jupyter-rsession-proxy=2.2.0 && \
    mamba clean --all

### Prints Jupyter server token when terminal is opened
COPY <<"EOF" ${HOME}/.get-jupyter-url.sh
jupyter_token () {
  jupyter server list 2>&1 \
  | grep -oP '(?<=token=)[[:alnum:]]*'
}
echo "Jupyter server token: $(jupyter_token)"
EOF
RUN echo "sh \${HOME}/.get-jupyter-url.sh" >> ${HOME}/.bashrc
