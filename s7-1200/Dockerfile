FROM ubuntu:18.04

USER root
WORKDIR /root/
RUN apt-get update && apt-get update --fix-missing && apt-get -y install lighttpd git python-pip autoconf bison build-essential pkg-config bison flex autoconf automake libtool make git python2.7 python-pip sqlite3 cmake sudo && pip install flask flask-login pyserial pymodbus


# Install dependencies
RUN apt-get install -y wget gcc make openssl libffi-dev libgdbm-dev libsqlite3-dev libssl-dev zlib1g-dev \
  && apt-get clean


WORKDIR /home/


RUN apt-get install -y python3.7 nano vim software-properties-common git libevent-dev libdumbnet-dev libpcap-dev libpcre3-dev libedit-dev bison flex libtool automake git zlib1g-dev make
RUN git clone https://github.com/marcolucc/Honeyd.git && git clone https://github.com/sefcom/honeyplc.git
RUN add-apt-repository ppa:deadsnakes/ppa

WORKDIR Honeyd
RUN mv configure.in configure.ac
RUN ./autogen.sh
RUN ./configure
RUN make 
RUN make install 

RUN echo "alias python=python3.7" >> ~/.bashrc
RUN export PATH=${PATH}:/usr/bin/python3.7
RUN /bin/bash -c "source ~/.bashrc"

# Install pip
RUN apt install python3-pip -y
RUN python3 -m pip install --upgrade pip
RUN pip install snmpsim==0.4.7 pyasn1==0.4.8

COPY ./data /data
 

# Copy the script to the image
COPY signatures.sh /app/signatures.sh
WORKDIR /app
ADD honeyd.conf /app/honeyd.conf

# Set the script as the command to run when the container starts
#RUN chmod +x signatures.sh && ./signatures.sh

WORKDIR /home/

RUN git clone https://github.com/thiagoralves/OpenPLC_v3.git
WORKDIR /home/OpenPLC_v3/
RUN ls -al install.sh
RUN sudo chmod +x install.sh
RUN sudo ./install.sh linux

RUN mkdir /home/OpenPLC_v3/scripts

COPY run.sh /home/OpenPLC_v3/
RUN sudo chmod +x run.sh
COPY PLC3.st /home/OpenPLC_v3/scripts
COPY launch.sh /home/OpenPLC_v3/launch.sh

COPY ./profile /var/www/html
COPY lighthttpd.conf /etc/lighttpd/lighttpd.conf

#snap7 
RUN cd /home/honeyplc/snap7/build/bin/x86_64-linux/ && cp libsnap7.so-1200 /usr/lib/libsnap7.so
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
EXPOSE 161
RUN useradd -ms /bin/bash newuser
COPY init.sh /home/init.sh
RUN sudo chmod +x /home/init.sh
CMD /home/init.sh
#CMD tail -f /dev/null
#CMD nohup lighttpd -D -f /etc/lighttpd/lighttpd.conf &> httpd.log; nohup honeyd -d -f /app/honeyd.conf &> honeyd.log; sh /home/OpenPLC_v3/run.sh; sudo bash /home/honeyplc/snap7/examples/cpp/x86_64-linux/server 0.0.0.0
#CMD sh /home/OpenPLC_v3/launch.sh
#WORKDIR /app
#CMD ["honeyd", "-d", "-f", "honeyd.conf"]