FROM debian:stable
MAINTAINER falconpioupiou <falconpioupiou@fake.local>

# Environment
ENV DEBIAN_FRONTEND noninteractive
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8
ENV LC_ALL C.UTF-8

# Get current
RUN apt-get update -y && apt-get dist-upgrade -y

# Install packages
RUN apt-get install -y wget apache2 libtinfo5

# Install vulnerable bash version from wayback/snapshot archive
#RUN wget http://snapshot.debian.org/archive/debian/20130101T091755Z/pool/main/b/bash/bash_4.2%2Bdfsg-0.1_amd64.deb -O /tmp/bash_4.2+dfsg-0.1_amd64.deb && \
# dpkg -i /tmp/bash_4.2+dfsg-0.1_amd64.deb

COPY bash_4.2_dfsg-0.1_amd64.deb /tmp/bash_4.2_dfsg-0.1_amd64.deb 
RUN dpkg -i /tmp/bash_4.2_dfsg-0.1_amd64.deb

# Setup vulnerable web content
COPY index.html /var/www/html/
# for the liveness layer of k8s
COPY index.html /var/www/ 
COPY vulnerable /usr/lib/cgi-bin/
RUN cd /etc/apache2/mods-enabled && ln -s ../mods-available/cgi.load
RUN chown www-data:www-data /usr/lib/cgi-bin/vulnerable 

# Removing the chmod +x because already done in the repo
# && \
#    chmod u+x /usr/lib/cgi-bin/vulnerable

# Clean up
RUN apt-get autoremove && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Expose the port for usage with the docker -P switch
EXPOSE 80

# Run Apache 2
CMD ["/usr/sbin/apache2ctl", "-DFOREGROUND"]

#
# Dockerfile for vulnerability as a service - CVE-2014-6271
#
