#
# RabbitMQ Dockerfile
#
# https://github.com/dockerfile/rabbitmq
#

# Pull base image.
FROM debian:jessie

# Add files.
ADD bin/rabbitmq-start /usr/local/bin/

RUN \
	apt-get update --fix-missing && apt-get install -y \
	wget 

# Install RabbitMQ.
RUN \
  wget -qO - https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | apt-key add - && \
  echo "deb http://www.rabbitmq.com/debian/ testing main" > /etc/apt/sources.list.d/rabbitmq.list && \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y rabbitmq-server && \
  rm -rf /var/lib/apt/lists/* && \
  rabbitmq-plugins enable rabbitmq_management && \
  rabbitmq-plugins enable rabbitmq_shovel && \
  rabbitmq-plugins enable rabbitmq_shovel_management && \
  echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config && \
  echo "[rabbitmq_management,rabbitmq_shovel,rabbitmq_shovel_management]." > /etc/rabbitmq/enabled_plugins && \  
  chmod +x /usr/local/bin/rabbitmq-start

# Define environment variables.
ENV RABBITMQ_LOG_BASE /data/log
ENV RABBITMQ_MNESIA_BASE /data/mnesia

# Define mount points.
VOLUME ["/data/log", "/data/mnesia"]

# Define working directory.
WORKDIR /data

# Import configs
RUN \
	apt-get update --fix-missing && apt-get install -y \
	python  curl
ADD rabbitmqbackup/rabbitmqadmin /usr/local/bin/
ADD rabbitmqbackup/rabbit-backup.config /etc/rabbitmq/
RUN chmod +x /usr/local/bin/rabbitmqadmin
#RUN /usr/local/bin/rabbitmqadmin -q import /etc/rabbitmq/rabbit-backup.config
# Define default command.
CMD ["rabbitmq-start"]

# Expose ports.
EXPOSE 5672
EXPOSE 15672


#####Rabbit add user 
#rabbitmqctl add_user admin password 