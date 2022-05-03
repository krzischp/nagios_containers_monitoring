FROM ubuntu

RUN apt update && apt install -y nagios-nrpe-server nagios-plugins nagios-nrpe-plugin vim

ENTRYPOINT ["tail", "-f", "/dev/null"]
