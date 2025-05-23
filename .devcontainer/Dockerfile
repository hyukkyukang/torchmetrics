# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.194.0/containers/python-3/.devcontainer/base.Dockerfile

# [Choice] Python version: 3, 3.9, 3.8, 3.7, 3.6
ARG VARIANT="3.9"
FROM mcr.microsoft.com/vscode/devcontainers/python:${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then \
      su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; \
    fi

COPY requirements/ /tmp/pip-tmp/requirements/
RUN \
    pip3 install awscli && \
    aws s3 sync --no-sign-request s3://sphinx-packages/ dist/  && \
    # trying to resolve pesq installation issue
    pip3 install -q "numpy<1.24" && \
    pip3 --disable-pip-version-check --no-cache-dir install \
      -r /tmp/pip-tmp/requirements/_devel.txt \
      -r /tmp/pip-tmp/requirements/_docs.txt \
      --extra-index-url="https://download.pytorch.org/whl/cpu/" \
      --find-links="dist/" && \
    rm -rf /tmp/pip-tmp

# [Optional] If your pip requirements rarely change, uncomment this section to add them to the image.
# RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements/base.txt \
#    && rm -rf /tmp/pip-tmp

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1
