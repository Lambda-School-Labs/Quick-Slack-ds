# syntax = docker/dockerfile:experimental
FROM python:3.7.6-buster

ENV INSTALL_PATH /api
RUN mkdir $INSTALL_PATH
WORKDIR $INSTALL_PATH

ENV PYTHONUNBUFFERED 1
ADD https://model-2.s3.us-east-2.amazonaws.com/medium_ft.pkl /datafiles/
ADD https://model-2.s3.us-east-2.amazonaws.com/distil-bert-SO.tar.gz /datafiles/
ADD https://model-2.s3.us-east-2.amazonaws.com/dim768-trees13.ann /datafiles/
ADD https://model-2.s3.us-east-2.amazonaws.com/tfidf.ann /datafiles/
ADD https://model-2.s3.us-east-2.amazonaws.com/tfidf.pkl /datafiles/ 
ADD https://model-2.s3.us-east-2.amazonaws.com/svd.pkl /datafiles/
ADD https://model-2.s3.us-east-2.amazonaws.com/hired.csv /datafiles/
ADD https://model-2.s3.us-east-2.amazonaws.com/tfidf_m_ids.csv /datafiles/
ADD https://model-2.s3.us-east-2.amazonaws.com/bert_m_ids.csv /datafiles/

COPY requirements.txt requirements.txt
RUN --mount=type=cache,target=/root/.cache/pip pip install -r requirements.txt
COPY . .
RUN useradd model -m
RUN chown model:model . -R
RUN chown model:model /datafiles -R
USER model
CMD gunicorn -c "python:config.gunicorn" "api.app:create_app()"