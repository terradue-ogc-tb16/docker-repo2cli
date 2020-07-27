## l0-binder-docker

Centos 7 base image for a customized docker for Binder

It has opensearch-client and ciop-copy installed.

Log on docker hub with

```bash
docker login registry-1.docker.io
```

Then build the image with:

```bash
docker build --tag registry-1.docker.io/terradue/l1-binder:latest .
```

Upload it with:

```bash
docker push registry-1.docker.io/terradue/l1-binder
```

Test it with:

```bash
docker run --rm -it -p 9010:8888 registry-1.docker.io/terradue/l1-binder jupyter notebook --port 8888 --ip=0.0.0.0 --NotebookApp.token='' --NotebookApp.password=''```

Open the browser at the port 9010

### Miniforge

`conda` is installed using miniforge. The version is defined in the `install-miniforge.bash` script:

```bash
MINIFORGE_VERSION=4.8.3-2
# SHA256 for installers can be obtained from https://github.com/conda-forge/miniforge/releases
SHA256SUM="4f897e503bd0edfb277524ca5b6a5b14ad818b3198c2f07a36858b7d88c928db"

URL="https://github.com/conda-forge/miniforge/releases/download/${MINIFORGE_VERSION}/Miniforge3-${MINIFORGE_VERSION}-Linux-x86_64.sh"
INSTALLER_PATH=/tmp/miniforge-installer.sh
```

The version is updated in this script. 

### YUM

The list of yum packages to install is defined in the file `yum.list`

If needed, add new yum packages in that file.

### Environments

There are two environments: 

- base
- notebook

The `notebook` environment content is defined by the file `environment.yml`.
Its content can be changed but the structure is:

```yaml
dependencies:
  - python=3.7
  - ipywidgets==7.5.1
  - jupyterlab==2.1.2
  - jupyterhub-singleuser==1.1.0
  - nbconvert==5.6.1
  - notebook==6.0.3
  - nteract_on_jupyter==2.1.3
  - widgetsnbextension 
  - ipyleaflet
  - nodejs
  - voila
  - conda-build
```  

It needed, add new conda packages to this list.

### Extensions

The jupyter `notebook` or `lab` extensions are defined in the `install-extensions.bash` script.

Below an example:

```bash
${NB_PYTHON_PREFIX}/bin/jupyter labextension install @jupyter-widgets/jupyterlab-manager@2.0

${NB_PYTHON_PREFIX}/bin/jupyter labextension install jupyter-leaflet
```

Add new extensions in this file. Make sure their installation and configuration is successful.

### Templates 

The templates for the `template` jupyter lab extention are managed by the `install-templates.bash` script.


### Issues

* kernel persistance is not guaranteed:

```bash
RUN mkdir -p /usr/local/share/jupyter && chown -R $NB_USER:$NB_GID /usr/local/share/jupyter
```



