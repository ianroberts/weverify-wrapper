FROM registry-medialab.afp.com/twintplus:0.12 as base

# install openjre:8
RUN mkdir -p /usr/share/man/man1 && apt-get update --allow-releaseinfo-change && apt install openjdk-11-jre -y && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

FROM base

COPY ./weverify-wrapper-webapp/target/weverify-wrapper-webapp.jar weverify-wrapper-webapp.jar 

ENV PROFILE=prod

ENTRYPOINT [ "/bin/bash", "-c", "java  -Djava.security.egd=file:/dev/./urandom -jar -Dspring.profiles.active=$PROFILE weverify-wrapper-webapp.jar"]

CMD ["/bin/bash"]