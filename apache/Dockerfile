FROM python:3.9
 
ARG PASS=Admin 

RUN apt-get update 
RUN apt-get install -y libexpat1 apache2 apache2-utils ssl-cert libapache2-mod-wsgi-py3  

RUN cd /usr/src && python -m venv WebScripts
RUN bash -c 'cd /usr/src/WebScripts && source bin/activate && pip install WebScripts --install-option "--owner=www-data" --install-option "--admin-password=${PASS}"'

WORKDIR /usr/src/WebScripts 

RUN mkdir -p /usr/src/WebScripts/logs/ 
RUN mkdir -p /usr/src/WebScripts/certificat/ 

RUN touch /usr/src/WebScripts/logs/apache-errors.logs 
RUN touch /usr/src/WebScripts/logs/apache-custom.logs 

COPY WebScripts.crt /usr/src/WebScripts/certificat/WebScripts.crt 
COPY WebScripts.pem /usr/src/WebScripts/certificat/WebScripts.pem 
COPY wsgi.conf /etc/apache2/conf-available/wsgi.conf

RUN echo "ServerSignature Off" >> /etc/apache2/apache2.conf
RUN echo "ServerTokens Prod" >> /etc/apache2/apache2.conf

RUN rm -f /etc/apache2/conf-available/serve-cgi-bin.conf
RUN rm -f /etc/apache2/conf-enabled/serve-cgi-bin.conf

RUN chown www-data:www-data /usr/src/WebScripts
RUN chown www-data:www-data /usr/src/WebScripts/logs

RUN sed -i "s/192.168.\*,172.16.\*,10.\*,127.0.\*/*/" /usr/src/WebScripts/lib/python3.9/site-packages/WebScripts/data/users.csv
VOLUME /usr/src/WebScripts/lib/python3.9/site-packages/WebScripts/data/

EXPOSE 80/tcp
EXPOSE 443/tcp

RUN a2enconf wsgi
RUN a2enmod ssl

ENTRYPOINT [ "apachectl", "-D", "FOREGROUND" ]
