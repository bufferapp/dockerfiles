FROM ubuntu

RUN apt-get update && \
apt-get install build-essential -y && \
apt-get install ntp libyaml-dev libevent-dev zlib1g zlib1g-dev openssl libssl-dev libxml2 libreadline-gplv2-dev curl wget unzip sudo -y

RUN mkdir ~/sources && \
cd ~/sources && \
curl -O ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p362.tar.gz && \
tar zxf ruby-1.9.3-p362.tar.gz && \
cd ruby-1.9.3-p362 && \
./configure && make && \
make install && \
gem install bundler

RUN cd ~/sources && \
curl -O http://download.redis.io/releases/redis-2.6.17.tar.gz && \
tar zxf redis-2.6.17.tar.gz && \
cd redis-2.6.17 && \
make && \
make install

RUN useradd -m ots && \
usermod -aG sudo ots && \
mkdir /etc/onetime && \
chown ots /etc/onetime

RUN echo "ots ALL=(ALL) NOPASSWD: ALL">>/etc/sudoers

USER ots
RUN cd && wget https://github.com/onetimesecret/onetimesecret/archive/master.zip && \
unzip master.zip && \
cd /home/ots/onetimesecret-master && \
bundle install --frozen --deployment --without=dev && \
bin/ots init && \
sudo mkdir /var/log/onetime /var/run/onetime /var/lib/onetime && \
sudo chown ots /var/log/onetime /var/run/onetime /var/lib/onetime && \
cp -R etc/* /etc/onetime/
COPY config/config /etc/onetime/config
COPY config/redis.conf /etc/onetime/redis.conf
RUN sudo chown ots /etc/onetime/config && sudo chgrp ots /etc/onetime/config && \
sudo chown ots /etc/onetime/redis.conf && sudo chgrp ots /etc/onetime/redis.conf


WORKDIR /home/ots/onetimesecret-master

EXPOSE 7143

CMD redis-server /etc/onetime/redis.conf && bundle exec thin -e dev -R config.ru -p 7143 start

