ARG TAG=22.02
FROM nvcr.io/nvidia/tensorrt:${TAG}-py3

# copy just the code needed to run the export script
ENV HOME=/home/docker HERMES=libs/hermes/hermes
COPY $HERMES.quiver $HOME/gw-iaas/$HERMES.quiver
COPY $HERMES.typeo $HOME/gw-iaas/$HERMES.typeo
COPY projects/gw-iaas/export $HOME/gw-iaas/projects/gw-iaas/export
WORKDIR $HOME/gw-iaas/projects/gw-iaas/export

# install poetry and use it to install
# export script dependencies to global
# Python environment, not virtual env.
# TODO: Something about doing a sudo install
# using poetry causes the poetry-installed
# libs to get installed at /usr/lib/python3.8/site-packages
# instead of either /usr/lib/python3.8/dist-packages,
# which I think happens because Poetry uses pip
# under the hood, or /usr/local/lib/python3.8/dist-packages,
# which I think is caused both by pip as well
# as the sudo nature of the install. Currently "solving"
# this with a lazy copy. Should probably either update
# the PYTHONPATH environment variable or possibly
# do a symlink instead?
ARG POETRY_VERSION=1.2.0a2
RUN set -x \
        \
        && python -m pip install --upgrade poetry==${POETRY_VERSION} \
        \
        && poetry config virtualenvs.create false \
        \
        && poetry install --without dev --no-interaction \
        \
        && cp -r \
            /usr/lib/python3.8/site-packages/* \
            /usr/local/lib/python3.8/dist-packages

ENTRYPOINT ["python", "export.py"]
CMD [""]
