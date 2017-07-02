# Base image
FROM hypriot/rpi-alpine:3.5

MAINTAINER Edwin Luijten "edwin.luijten@live.nl"	

RUN apk add --update --no-cache --quiet \
    bash \
    curl \
	openjdk8-jre-base

# Neo4j
ENV NEO4J_SHA256=24fd6a704e0d80c4b4f9a3d17ce0db23f258a8cdcfa1eb28d7803b7d1811ee96 \
    NEO4J_TARBALL=neo4j-community-3.2.1-unix.tar.gz
ARG NEO4J_URI=http://dist.neo4j.org/neo4j-community-3.2.1-unix.tar.gz

RUN curl --fail --silent --show-error --location --remote-name ${NEO4J_URI} \
    && echo "${NEO4J_SHA256}  ${NEO4J_TARBALL}" | sha256sum -csw - \
    && tar --extract --file ${NEO4J_TARBALL} --directory /var/lib \
    && mv /var/lib/neo4j-* /var/lib/neo4j \
    && rm ${NEO4J_TARBALL} \
    && mv /var/lib/neo4j/data /data \
    && ln -s /data /var/lib/neo4j/data \
    && apk del curl

WORKDIR /var/lib/neo4j

VOLUME /data

COPY docker-entrypoint /usr/local/bin/

RUN chmod -R 755 /usr/local/bin/docker-entrypoint

EXPOSE 7474 7473 7687

ENTRYPOINT ["docker-entrypoint"]
CMD ["neo4j"]