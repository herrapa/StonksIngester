FROM telegraf:latest
COPY ./plugin .
RUN pwd
RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends python3 python3-pip python3-setuptools python3-wheel && \
    rm -rf /var/lib/apt/lists/*
RUN pip3 install --no-cache-dir -r requirements.txt
EXPOSE 8125

