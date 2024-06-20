FROM quay.io/jupyter/datascience-notebook:r-4.3.1

### Environment variables
ENV R_STUDIO_VERSION 2023.12.1-402

###########################
### SYSTEM INSTALLATION ###
###########################
USER root

### System dependencies. Feel free to add packages as necessary.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        # Basic system usage
        lmodern \
        file \
        curl \
        # g++ \
        # tmux \
        # Dependencies for RStudio
        psmisc \
        lsb-release \
        libssl-dev \
        libclang-dev \
        libpq5 \
        libtiff-dev \
        && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/* /tmp/library-scripts

### R compiler settings
# RUN R -e "dotR <- file.path(Sys.getenv('HOME'), '.R'); if(!file.exists(dotR)){ dir.create(dotR) }; Makevars <- file.path(dotR, 'Makevars'); if (!file.exists(Makevars)){  file.create(Makevars) }; cat('\nCXX14FLAGS=-O3 -fPIC -Wno-unused-variable -Wno-unused-function', 'CXX14 = g++ -std=c++1y -fPIC', 'CXX = g++', 'CXX11 = g++', file = Makevars, sep = '\n', append = TRUE)"

### CRAN mirror
# RUN R -e "dotRprofile <- file.path(Sys.getenv('HOME'), '.Rprofile'); if(!file.exists(dotRprofile)){ file.create(dotRprofile) }; cat('local({r <- getOption(\"repos\")', 'r[\"CRAN\"] <- \"https://cloud.r-project.org\"', 'options(repos=r)', '})', file = dotRprofile, sep = '\n', append = TRUE)"

### RStudio from source
RUN wget -q https://download2.rstudio.org/server/jammy/amd64/rstudio-server-${R_STUDIO_VERSION}-amd64.deb && \
    apt-get install -yq --no-install-recommends ./rstudio*.deb && \
    rm -f ./rstudio*.deb && \
    apt-get clean && \
    chmod 777 /var/run/rstudio-server && \
    chmod +t /var/run/rstudio-server

#########################
### USER INSTALLATION ###
#########################
USER ${NB_USER}

### Anaconda (conda/mamba) packages
RUN mamba install -y -c conda-forge --freeze-installed \
        # Jupyter setup
        jupyter-rsession-proxy=2.2.0 \
        #######################################################
        ### Add your own conda dependencies installed with  ###
        ### `conda/mamba` as needed below this comment      ###
        ### Example (note the backslash after name):        ###
        ### scikit-learn \                                  ###
        #######################################################
        && \
    mamba clean --all

### PyPI (pip) packages
# RUN pip install \ 
#         radian==0.6.11

### R packages
    # Versioned
# RUN R -q -e 'remotes::install_version("markdown", version="1.12", repos="cloud.r-project.org")' && \
#     R -q -e 'remotes::install_version("languageserver", version="0.3.16", repos="cloud.r-project.org")' && \
#     R -q -e 'remotes::install_version("httpgd", version="2.0.1", repos="cloud.r-project.org")' && \
#     # Latest Dev Versions
#     R -q -e 'remotes::install_github("ManuelHentschel/vscDebugger")'

### Prints Jupyter server token when terminal is opened
RUN echo "echo \"Jupyter server token: \$(jupyter server list 2>&1 | grep -oP '(?<=token=)[[:alnum:]]*')\"" > ${HOME}/.get-jupyter-url.sh && \
    echo "sh \${HOME}/.get-jupyter-url.sh" >> ${HOME}/.bashrc