# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0


FROM python:3.12-slim-bookworm as base

FROM base as builder
RUN apt-get -qq update \
    && apt-get install -y --no-install-recommends g++ \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory before copying
WORKDIR /usr/src/app/
COPY ./src/loadgenerator/requirements.txt .
RUN pip install --prefix="/reqs" -r requirements.txt

FROM base
WORKDIR /usr/src/app/
COPY --from=builder /reqs /usr/local
COPY ./src/loadgenerator/locustfile.py .
COPY ./src/loadgenerator/people.json .
ENV LOCUST_PLAYWRIGHT=1
ENV PLAYWRIGHT_BROWSERS_PATH=/opt/pw-browsers
RUN playwright install --with-deps chromium
ENTRYPOINT locust --skip-log-setup
