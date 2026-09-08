FROM public.ecr.aws/docker/library/python:3.10-alpine
ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /
RUN adduser -u 1000 -D app
RUN apk update && \
  apk add \
  pcre \
  pcre-dev \
  build-base \
  gcc \
  linux-headers \
  openssl \
  libffi-dev \
  && pip install pipenv

COPY Pipfile* /app/
RUN cd /app && (pipenv install --system --skip-lock || pip install --no-cache-dir flask flask-cors uwsgi pytest)


COPY . /app
WORKDIR /app

CMD uwsgi --http :5000 --master --enable-threads -s /var/www/app/app.sock --manage-script-name --mount /=app:app