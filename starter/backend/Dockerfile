FROM public.ecr.aws/docker/library/python:3.10-slim-bookworm
ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /
RUN adduser --uid 1000 --disabled-login --disabled-password app
RUN apt-get update && \
    apt-get install -y \
    libpcre3 \
    libpcre3-dev \
    build-essential \
    gcc \
    libssl-dev \
    libffi-dev \
    python3-dev \
    && pip install pipenv \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile* /app/
RUN cd /app && pipenv install --system --deploy

COPY . /app

CMD uwsgi --http :5000 --master --enable-threads -s /var/www/app/app.sock --manage-script-name --mount /=app:app