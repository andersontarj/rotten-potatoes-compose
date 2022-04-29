FROM python:3.8.13-slim-buster
LABEL MAINTAINER = "Anderson Amaral - INICIATIVA KUBERNETES"

WORKDIR /app
COPY requirements.txt .
RUN python -m pip install -r requirements.txt
COPY . /app

EXPOSE 5000
CMD ["gunicorn", "--workers=3", "--bind", "0.0.0.0:5000", "-c","config.py", "app:app"]