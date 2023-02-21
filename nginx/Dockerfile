FROM python:3.11

ARG PASS=Admin

RUN apt-get update
RUN apt-get install -y nginx sudo

RUN useradd --system --no-create-home --shell /bin/false WebScripts

RUN cd /usr/src && python -m venv WebScripts
RUN bash -c 'cd /usr/src/WebScripts && source bin/activate && pip install WebScripts --install-option "--owner=WebScripts" --install-option "--admin-password=${PASS}" && python -m WebScripts.harden -p "${PASS}" -o "WebScripts" -d "."'

WORKDIR /usr/src/WebScripts

RUN mkdir -p /usr/src/WebScripts/logs/
RUN chown WebScripts:WebScripts /usr/src/WebScripts/logs/

RUN mkdir -p /usr/src/WebScripts/certificat/
COPY WebScripts.crt /usr/src/WebScripts/certificat/WebScripts.crt
COPY WebScripts.pem /usr/src/WebScripts/certificat/WebScripts.pem

COPY WebScripts.service /lib/systemd/system/WebScripts.service
COPY WebScripts.conf /etc/nginx/sites-available/WebScripts.conf

RUN sed -i 's/"webproxy_number": 0,/"webproxy_number": 1,/' /usr/src/WebScripts/lib/python3.11/site-packages/WebScripts/config/server.json

RUN rm /etc/nginx/sites-enabled/default
RUN sed -i "s/# server_tokens off;/server_tokens off;/" /etc/nginx/nginx.conf
RUN ln -s /etc/nginx/sites-available/WebScripts.conf /etc/nginx/sites-enabled

RUN sed -i "s/192.168.\*,172.16.\*,10.\*,127.0.\*/*/" /usr/src/WebScripts/data/users.csv
VOLUME /usr/src/WebScripts/data/

EXPOSE 80/tcp
EXPOSE 443/tcp

RUN echo "nginx -g 'daemon off;' &" > /bin/launcher.sh
RUN echo "sudo -u WebScripts bin/python3 -m WebScripts" >> /bin/launcher.sh

RUN chown root:root /usr/src/WebScripts

ENTRYPOINT [ "bash", "/bin/launcher.sh" ]

