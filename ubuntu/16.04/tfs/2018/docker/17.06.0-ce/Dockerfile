FROM microsoft/vsts-agent:ubuntu-16.04-docker-17.06.0-ce

RUN set -x \
 && curl -fSL https://github.com/Microsoft/vsts-agent/releases/download/v2.122.1/vsts-agent-ubuntu.16.04-x64-2.122.1.tar.gz -o agent.tgz \
 && mkdir agent \
 && cd agent \
 && tar -xzf ../agent.tgz \
 && chown -R root:root . \
 && cd .. \
 && rm agent.tgz

COPY ./start.sh .
RUN chmod +x start.sh

CMD ["./start.sh"]
