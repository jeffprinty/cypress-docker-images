FROM cypress/base:10

USER root

RUN node --version
RUN echo "force new chrome here"

# install Chromebrowser
RUN \
  wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
  echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
  apt-get update && \
  apt-get install -y dbus-x11 google-chrome-stable golang-go && \
  rm -rf /var/lib/apt/lists/*

# "fake" dbus address to prevent errors
# https://github.com/SeleniumHQ/docker-selenium/issues/87
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null

# Add zip utility - it comes in very handy
RUN apt-get update && apt-get install -y zip
WORKDIR /tmp

# Add consul cli for retrieving configuration
RUN curl -so consul.zip https://releases.hashicorp.com/consul/1.4.2/consul_1.4.2_linux_amd64.zip
RUN unzip consul.zip
RUN ls -la
RUN mv consul /usr/local/bin/consul && \
  chmod +x /usr/local/bin/consul

# versions of local tools
RUN consul -v
RUN node -v
RUN npm -v
RUN yarn -v
RUN google-chrome --version
RUN zip --version
RUN git --version

# a few environment variables to make NPM installs easier
# good colors for most applications
ENV TERM xterm
# avoid million NPM install messages
ENV npm_config_loglevel warn
# allow installing when the main user is root
ENV npm_config_unsafe_perm true
