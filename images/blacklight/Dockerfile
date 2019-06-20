# A Dockerfile for Blacklight
FROM ubuntu:14.04

MAINTAINER David Jiao <djiao@iu.edu>

# Install Ruby and nodejs
RUN apt-get update
RUN apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev  python-software-properties libffi-dev liblzma-dev libgmp-dev -y
RUN apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev -y
RUN gpg --keyserver hkp://keys.gnupg.net:80 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
RUN curl -L https://get.rvm.io | sed 's/hkp:\/\/pool.sks-keyservers.net\(.*9DC0E3.*\)/hkps:\/\/pool.sks-keyservers.net:80\1/' | bash -s stable
RUN /bin/bash -c "source /usr/local/rvm/scripts/rvm"
RUN /bin/bash -l -c "rvm install 2.3.1 --no-docs"
RUN /bin/bash -l -c "rvm use 2.3.1 --default --no-docs"
RUN curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
RUN apt-get install -y nodejs
ENV PATH /usr/local/rvm/gems/ruby-2.3.1/bin:/usr/local/rvm/gems/ruby-2.3.1@global/bin:/usr/local/rvm/rubies/ruby-2.3.1/bin:$PATH:/usr/local/rvm/bin

RUN gem install bundler

# # Install Rails
RUN gem install rails

# Download and install blacklight
WORKDIR /opt
RUN rails new search_app -m https://raw.github.com/projectblacklight/blacklight/master/template.demo.rb

# Add default env variable referencing our solr container
ENV SOLR_URL http://solr:8983/solr/blacklight

WORKDIR /opt/search_app
# RUN rake jetty:start
#RUN rake solr:marc:index_test_datas

# Upgrade nodejs
WORKDIR /opt
RUN apt-get install wget
RUN wget https://nodejs.org/dist/v6.11.3/node-v6.11.3-linux-x64.tar.gz
RUN mkdir -p /opt/nodejs
RUN tar -xvzf node-v6.11.3-linux-x64.tar.gz -C /opt/nodejs/
WORKDIR /opt/nodejs
RUN mv node-v6.11.3-linux-x64 6.11.3
RUN ln -s 6.11.3 current
RUN touch /usr/bin/node && rm /usr/bin/node
RUN ln -s /opt/nodejs/current/bin/node /usr/bin/node

WORKDIR /opt/search_app
CMD bundle exec rails s -p 3000 -b '0.0.0.0'
