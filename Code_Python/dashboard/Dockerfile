ARG PYTHON_VERSION=3.8
ARG DEBIAN_VERSION=bullseye

# Build virtualenv
FROM python:${PYTHON_VERSION}-${DEBIAN_VERSION} AS BUILD

WORKDIR /usr/src/app

# Build dependencies
COPY requirements.txt .

RUN set -xe && \
    python -m venv --symlinks --prompt qmin ./venv && \
    ./venv/bin/pip install --upgrade pip setuptools wheel && \
    ./venv/bin/python -m pip install --no-cache-dir -r requirements.txt


# Create release image
ARG PYTHON_VERSION
ARG DEBIAN_VERSION
FROM python:${PYTHON_VERSION}-slim-${DEBIAN_VERSION} AS RELEASE

WORKDIR /usr/src/app

# Copy statements
COPY --from=BUILD /usr/src/app/venv ./venv
COPY . .

RUN chmod 755 ./scripts/start-qmin.sh

ENV PATH=/usr/src/app/venv/bin:${PATH} \
    PYTHONUNBUFFERED=1

EXPOSE 8000

CMD [ "./scripts/start-qmin.sh" ]
