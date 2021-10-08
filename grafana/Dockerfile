FROM frapsoft/openssl:latest as ssl_generator

# See Github page for more details
# https://gist.github.com/thbkrkr/aa16435cb6c183e55a33
WORKDIR /workdir
RUN mkdir /artifacts && \
openssl rand -base64 48 > passphrase.txt && \
openssl genrsa -aes256 -passout file:passphrase.txt -out server.key 4096 && \
openssl req -new -passin file:passphrase.txt -key server.key -out server.csr -subj "/C=FR/O=dockerd/OU=Domain Control Validated/CN=*.dockerd.local" && \
cp server.key server.key.org && \
openssl rsa -in server.key.org -passin file:passphrase.txt -out server.key && \
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt && \
cp server.crt /artifacts/grafana.crt && \
cp server.key /artifacts/grafana.key


FROM grafana/grafana:8.2.0


COPY --from=ssl_generator /artifacts/* /etc/ssl/

CMD ["/usr/sbin/grafana-server", "-config", "/etc/grafana.ini", "-homepath", "/usr/share/grafana/"]

