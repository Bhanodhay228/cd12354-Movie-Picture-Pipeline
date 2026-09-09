FROM public.ecr.aws/docker/library/python:3.10-slim
ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /
RUN useradd --create-home --uid 1000 app
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
  build-essential \
  libpcre3-dev \
  libssl-dev \
  libffi-dev \
  && rm -rf /var/lib/apt/lists/*
RUN python -m pip install --no-cache-dir --upgrade pip setuptools wheel \
  && python -m pip install --no-cache-dir \
  flask==2.2.3 \
  flask-cors==3.0.10 \
  uwsgi==2.0.21

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile* /app

COPY . /app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]