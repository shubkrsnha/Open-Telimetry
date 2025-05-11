FROM python:3.11 AS base
# or: FROM python:3.11-slim if you want a smaller image, but full version is safer

WORKDIR /usr/src/app

COPY requirements.txt .

RUN pip install --upgrade pip && \
    pip install -r requirements.txt

COPY . .

RUN pip install opentelemetry-instrumentation

ENV RECOMMENDATION_PORT=1010

ENTRYPOINT ["python", "recommendation_server.py"]
