FROM quay.io/jupyter/datascience-notebook:r-4.3.1

### Environment variables
# ENV R_STUDIO_VERSION 2023.12.1-402

###########################
### SYSTEM INSTALLATION ###
###########################
USER root

# ### System dependencies. Feel free to add packages as necessary.
# RUN apt-get update && \
#     apt-get install -y --no-install-recommends \
#         # Basic system usage
#         lmodern \
#         file \
#         # Dependencies for RStudio
#         psmisc \
#         lsb-release \
#         libssl-dev \
#         libclang-dev \
#         libpq5 \
#         libtiff-dev \
#         && \
#     apt-get clean -y && \
#     rm -rf /var/lib/apt/lists/* /tmp/library-scripts
# 
# ### RStudio Server
# RUN wget -q https://download2.rstudio.org/server/jammy/amd64/rstudio-server-${R_STUDIO_VERSION}-amd64.deb && \
#     apt-get install -yq --no-install-recommends ./rstudio*.deb && \
#     rm -f ./rstudio*.deb && \
#     apt-get clean && \
#     chmod 777 /var/run/rstudio-server && \
#     chmod +t /var/run/rstudio-server

#########################
### USER INSTALLATION ###
#########################
USER ${NB_USER}

# ### RStudio launch button in Jupyter Lab
# RUN mamba install -y -c conda-forge --freeze-installed \
#         jupyter-rsession-proxy=2.2.0 \
#         && \
#     mamba clean --all
# 
### Prints Jupyter server token when terminal is opened
RUN echo "echo \"Jupyter server token: \$(jupyter server list 2>&1 | grep -oP '(?<=token=)[[:alnum:]]*')\"" > ${HOME}/.get-jupyter-url.sh && \
    echo "sh \${HOME}/.get-jupyter-url.sh" >> ${HOME}/.bashrc
