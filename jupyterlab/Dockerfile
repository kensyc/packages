# Build the virtual environment
FROM debian:11-slim AS build
RUN apt-get update && \
    apt-get install --no-install-suggests --no-install-recommends --yes python3-venv gcc libpython3-dev && \
    python3 -m venv /venv && \
    /venv/bin/pip install --upgrade pip setuptools wheel

# Build the virtualenv as a separate step: put requirements on a separate layer
FROM build AS build-venv
RUN /venv/bin/pip install --disable-pip-version-check \
    jupyterlab \
    jupyterlab-vim \
    sklearn \
    numpy \
    pandas

# Copy the virtualenv into a distroless image
FROM gcr.io/distroless/python3-debian11
COPY --from=build-venv /venv /venv
WORKDIR /app
ENTRYPOINT ["/venv/bin/jupyter", "lab", "--ip=0.0.0.0","--port=8888", "--allow-root", "--no-browser"]
