FROM phusion/baseimage

MAINTAINER Ajish George <yell@aji.sh>

ENV DEBIAN_FRONTEND noninteractive

# Superset version
ARG SUPERSET_VERSION=0.20.4
ARG DREMIO_VERSION=1.3.11.1034
# Configure environment
ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    PYTHONPATH=/etc/superset:$PYTHONPATH \
    SUPERSET_VERSION=${SUPERSET_VERSION} \
    SUPERSET_HOME=/home/superset

# Create superset user & install dependencies
RUN useradd -U -m superset \
        && apt-get update \ 
        && apt-get install -y \
            build-essential \
            curl \
            git \
            libmysqlclient-dev \
            libldap2-dev \
            libpq-dev \
            libsasl2-dev \
            libssl-dev \
            openjdk-8-jdk \
            python3-dev \ 
            python3-pyodbc \
            python3-pip \
            alien \
            dpkg-dev \
            debhelper \
            build-essential \
        &&  curl -O http://download.dremio.com/odbc-driver/${DREMIO_VERSION}/dremio-odbc-${DREMIO_VERSION}-1.x86_64.rpm \
        && alien -i dremio-odbc-${DREMIO_VERSION}-1.x86_64.rpm \
        && pip3 install --no-cache-dir --upgrade pip \
        && pip3 install --no-cache-dir \
            flask-mail>=0.9.1 \
            flask-oauth>=0.12 \
            flask_oauthlib>=0.9.3 \
            impyla>=0.14.0 \
            mysqlclient>=1.3.7 \
            psycopg2>=2.6.1 \
            pyathenajdbc>=1.2.0 \
            pyhive>=0.5.0 \
            pyldap>=2.4.28 \
            redis>=2.10.5 \
            sqlalchemy-redshift>=0.7.0 \
            sqlalchemy-clickhouse>=0.1.1.post3 \
            Werkzeug>=0.12.1 \
            pyodbc>=4.0 \
            pytest>=3.2.3 \
            superset>=${SUPERSET_VERSION} \
            git+https://github.com/luzfcb/sqlalchemy-access.git \
        && apt-get purge --auto-remove -y gcc g++ make gfortran \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/* 

# Configure Filesystem
COPY superset /usr/local/bin
VOLUME /etc/superset
WORKDIR /home/superset

# Deploy application
EXPOSE 8088
HEALTHCHECK CMD ["curl", "-f", "http://localhost:8088/health"]
ENTRYPOINT ["superset"]
CMD ["runserver"]
USER superset
