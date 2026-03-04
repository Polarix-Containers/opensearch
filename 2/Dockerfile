ARG VERSION=2
ARG UID=200021
ARG GID=200021

FROM alpine:latest

LABEL maintainer="Thien Tran contact@tommytran.io"

ARG UID
ARG GID

ARG DISABLE_INSTALL_DEMO_CONFIG=true
ARG DISABLE_SECURITY_PLUGIN=false

ENV OPENSEARCH_HOME=/usr/share/opensearch
ENV JAVA_HOME=$OPENSEARCH_HOME/jdk
ENV PATH=$PATH:$JAVA_HOME/bin:$OPENSEARCH_HOME/bin
ENV LD_LIBRARY_PATH="$OPENSEARCH_HOME/plugins/opensearch-knn/lib"

RUN apk -U upgrade \
    && apk add bash libstdc++ sed \
    && rm -rf /var/cache/apk/*

RUN --network=none \
    addgroup -g ${GID} opensearch \
    && adduser -u ${UID} --ingroup opensearch --disabled-password --system opensearch --home ${OPENSEARCH_HOME}

COPY --from=opensearchproject/opensearch:2 --chown=$UID:$GID /usr/share/opensearch ${OPENSEARCH_HOME}

USER opensearch
WORKDIR ${OPENSEARCH_HOME}

RUN ./opensearch-onetime-setup.sh

EXPOSE 9200/tcp 9300/tcp 9600/tcp 9650/tcp

# CMD to run
ENTRYPOINT ["./opensearch-docker-entrypoint.sh"]
CMD ["opensearch"]