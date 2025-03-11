FROM ubuntu:20.04
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get install -y apt-utils

# Install requirements for Paramiko (Cryptography) library for SFTP
RUN apt-get install -y build-essential libssl-dev libffi-dev

# Install Python and Basic Python Tools
RUN apt-get install -y python3.9
RUN apt-get install -y python3.9-dev
RUN apt-get install -y python3.9-distribute
RUN apt-get install -y python3.9-pip
RUN python3.9 -m pip install --upgrade pip

# Copy requirements from source
COPY ./etc /requirements
RUN python3.9 -m pip install -r /requirements/requirements.txt

# Install additional/optional packages
RUN python3.9 -m pip install suds-jurko==0.6
RUN python3.9 -m pip install paramiko==2.0.2
RUN python3.9 -m pip install pycrypto==2.6.1
RUN python3.9 -m pip install supervisor==3.3.1
RUN python3.9 -m pip install m3-cdecimal==2.3

# Copy complete source
COPY . /bots
WORKDIR /bots

# Install BOTS and create directories
RUN python3.9 -m pip install /bots
RUN mkdir /usr/local/lib/python3.9/dist-packages/bots/botssys
RUN mkdir /usr/local/lib/python3.9/dist-packages/bots/botssys/sqlitedb
RUN mkdir /dirmon
COPY ./src/bots/install/bots.ini /usr/local/lib/python3.9/dist-packages/bots/config/
COPY ./src/bots/install/settings.py /usr/local/lib/python3.9/dist-packages/bots/config/
COPY ./src/bots/install/botsdb /usr/local/lib/python3.9/dist-packages/bots/botssys/sqlitedb/

COPY ./compose/bots_complete/postinstall.py /bots
RUN python3.9 /bots/postinstall.py


# Install DevCron
# failed, missing hg/mercurial : RUN python3.9 -m pip install -e hg+https://bitbucket.org/dbenamy/devcron#egg=devcron
RUN python3.9 -m pip install devcron

# Copy Supervisord.conf file
COPY ./compose/bots_complete/supervisord.conf /etc/supervisor/supervisord.conf

# Add Crontab file
COPY ./compose/bots_complete/crontab /usr/local/lib/python3.9/dist-packages/bots/config/

CMD [ "/usr/local/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf" ]

EXPOSE 8080
EXPOSE 9001
