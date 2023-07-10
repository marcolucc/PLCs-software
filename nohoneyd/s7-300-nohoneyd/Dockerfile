FROM ubuntu:18.04

USER root
WORKDIR /root/
RUN apt-get update && \
    apt-get -y install lighttpd git python-pip autoconf bison build-essential pkg-config bison flex autoconf automake libtool make git python2.7 python-pip sqlite3 cmake sudo
RUN pip install flask flask-login pyserial pymodbus EasyModbus==1.2.5


# Install dependencies
RUN apt-get update \
  && apt-get install -y wget gcc make openssl libffi-dev libgdbm-dev libsqlite3-dev libssl-dev zlib1g-dev \
  && apt-get clean


WORKDIR /home/

RUN git clone https://github.com/sefcom/honeyplc.git



WORKDIR /app

ADD comm_modbus.py /app/comm_modbus.py


WORKDIR /home/

RUN git clone https://github.com/thiagoralves/OpenPLC_v3.git
WORKDIR /home/OpenPLC_v3/
RUN ls -al install.sh
RUN sudo chmod +x install.sh
RUN sudo ./install.sh linux

RUN mkdir /home/OpenPLC_v3/scripts

COPY run.sh /home/OpenPLC_v3/
RUN sudo chmod +x run.sh
COPY PLC2.st /home/OpenPLC_v3/scripts
COPY launch.sh /home/OpenPLC_v3/launch.sh

COPY ./profile /var/www/html
COPY lighthttpd.conf /etc/lighttpd/lighttpd.conf

#snap7 
RUN cd /home/honeyplc/snap7/build/bin/x86_64-linux/ && cp libsnap7.so-300 /usr/lib/libsnap7.so
RUN cd /home/honeyplc/snap7/examples/cpp/x86_64-linux  

EXPOSE 502
EXPOSE 6668/udp
EXPOSE 8080
EXPOSE 8081
# Ports
EXPOSE 47
EXPOSE 67
EXPOSE 68
EXPOSE 80
EXPOSE 102

COPY init.sh /home/init.sh
RUN sudo chmod +x /home/init.sh
CMD /home/init.sh

#CMD nohup lighttpd -D -f /etc/lighttpd/lighttpd.conf &> httpd.log; nohup honeyd -d -f /app/honeyd.conf &> honeyd.log; nohup python /app/comm_modbus.py &> comm.log; sh /home/OpenPLC_v3/run.sh
#CMD python /app/comm_modbus.py
#CMD sh /home/OpenPLC_v3/launch.sh
#WORKDIR /app
#CMD ["honeyd", "-d", "-f", "honeyd.conf"]