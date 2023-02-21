FROM python:3.11

ARG PASS=Admin

RUN apt-get update
RUN apt-get install -y sudo

RUN useradd --system --no-create-home --shell /bin/false WebScripts

RUN cd /usr/src && python -m venv WebScripts
RUN bash -c 'cd /usr/src/WebScripts && source bin/activate && pip install WebScripts --install-option "--owner=WebScripts" --install-option "--admin-password=${PASS}" && python -m WebScripts.harden -p "${PASS}" -o "WebScripts" -d "."'

WORKDIR /usr/src/WebScripts

RUN mkdir -p /usr/src/WebScripts/logs/
RUN chown WebScripts:WebScripts /usr/src/WebScripts/logs/

RUN chown root:root /usr/src/WebScripts

RUN sed -i "s/192.168.\*,172.16.\*,10.\*,127.0.\*/*/" /usr/src/WebScripts/data/users.csv
VOLUME /usr/src/WebScripts/data/

EXPOSE 80/tcp

ENTRYPOINT [ "sudo", "-u", "WebScripts", "bin/python3", "-m", "WebScripts", "-i", "" ]

