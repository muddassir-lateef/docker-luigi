# Base luigi Dockerfile with the latest Debian version and Python 3.5.2
FROM debian:bullseye-slim

MAINTAINER Spiro Sideris <spirosideris@gmail.com>

# Update the system and install required packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3-dev \
        python3-pip \
        sudo \
        cron \
        libpq-dev \
        build-essential \
        postgresql-client \
    && \
    useradd --password NP --create-home --shell /bin/bash luigi && \
    echo "luigi ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Upgrade pip and install required Python packages
RUN python3 -m pip install --no-cache-dir --upgrade pip && \
    python3 -m pip install --no-cache-dir \
        'luigi==2.4.0' \
        'sqlalchemy==1.1.0' \
        'psycopg2==2.9.1'

RUN mkdir /etc/luigi && chmod -R a+rw /etc/luigi

COPY scripts/ /
RUN chmod +x /generate_config.py && chmod +x /run.sh && mv /run.sh /usr/bin

USER luigi

CMD ["run.sh"]
