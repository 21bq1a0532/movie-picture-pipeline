FROM public.ecr.aws/docker/library/python:3.10-alpine
ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /
RUN adduser -u 1000 -D app
RUN apk update && \
  apk add --no-cache \
  pcre \
  pcre-dev \
  build-base \
  gcc \
  linux-headers \
  openssl-dev \
  libffi-dev \
  && pip install --no-cache-dir pipenv

RUN mkdir -p /var/www/app && mkdir /app
COPY Pipfile* /app/
RUN cd /app && (pipenv install --system --skip-lock || pip install --no-cache-dir flask flask-cors uwsgi pytest)

ENV PYTHONPATH=/

COPY . /app
WORKDIR /

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "--pythonpath", "/", "--mount", "/=app:app", "--manage-script-name"]