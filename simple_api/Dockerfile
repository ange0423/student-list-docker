FROM python:3.11-slim

LABEL maintainer="FODING KAMDOM ANGE MICHELE <angemichelefoding@gmail.com>"

WORKDIR /

RUN apt-get update -y && apt-get install -y --no-install-recommends \
    gcc \
    python3-dev \
    libsasl2-dev \
    libldap2-dev \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt /requirements.txt
RUN pip3 install --no-cache-dir -r /requirements.txt

COPY student_age.py /student_age.py

RUN mkdir -p /data
VOLUME ["/data"]

EXPOSE 5000

CMD ["python3", "/student_age.py"]
