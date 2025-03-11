FROM python:3.9

ENV PYTHONUNBUFFERED 1

COPY ./etc /requirements

RUN pip install --upgrade pip
RUN pip install -r /requirements/requirements.txt

COPY . /bots
WORKDIR /bots

RUN pip install /bots
RUN mkdir /usr/local/lib/python3.9/site-packages/bots/botssys
RUN mkdir /usr/local/lib/python3.9/site-packages/bots/botssys/sqlitedb
COPY ./src/bots/install/bots.ini /usr/local/lib/python3.9/site-packages/bots/config/
COPY ./src/bots/install/settings.py /usr/local/lib/python3.9/site-packages/bots/config/
COPY ./src/bots/install/botsdb /usr/local/lib/python3.9/site-packages/bots/botssys/sqlitedb/

CMD [ "python3.9", "-u", "/usr/local/bin/bots-webserver.py" ]
EXPOSE 8080
