FROM ubuntu:xenial

MAINTAINER Xingjian Xu "xuxingjian@big.ac.cn"

RUN echo "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/" >> /etc/apt/sources.list
RUN gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
RUN gpg -a --export E084DAB9 | apt-key add -

RUN apt-get -qq update
RUN apt-get -yqq install build-essential curl wget git unzip
RUN apt-get -yqq install libcurl4-gnutls-dev libxml2-dev libssl-dev

RUN apt-get -yqq install python python-pip python-numpy python-scipy python-matplotlib python-pandas python-sympy
RUN pip install --upgrade pip
RUN pip install biopython

RUN apt-get -yqq install ruby

RUN apt-get -yqq install r-base r-base-dev
RUN R -e "install.packages('devtools', repos='http://cran.rstudio.com/')"
RUN R -e "source('https://bioconductor.org/biocLite.R');biocLite();"
RUN R -e "source('https://bioconductor.org/biocLite.R');biocLite('ballgown');"

# Install Bioperl
RUN apt-get install -yqq openssl libdb-dev zlib1g-dev graphviz cpanminus
RUN cpanm \
  CPAN::Meta \
  YAML \
  Digest::SHA \
  Module::Build \
  Test::Most \
  Test::Weaken \
  Test::Memory::Cycle \
  Clone

RUN cpanm \
  IO::Socket::INET6 \
  LWP \
  LWP::Protocol::https

# Install packages for XML processing
RUN apt-get install --yes \
  expat \
  libexpat-dev \
  libxml2-dev \
  libxslt1-dev \
  libgdbm-dev

RUN cpanm \
  XML::Parser \
  XML::Parser::PerlSAX \
  XML::DOM \
  XML::DOM::XPath \
  XML::SAX \
  XML::SAX::Writer \
  XML::Simple \
  XML::Twig \
  XML::Writer \
  XML::LibXML \
  XML::LibXSLT

# Install what counts as BioPerl dependencies
RUN cpanm \
  HTML::TableExtract \
  Algorithm::Munkres \
  Array::Compare \
  Convert::Binary::C \
  Error \
  Graph \
  GraphViz \
  Inline::C \
  PostScript::TextBlock \
  Set::Scalar \
  Sort::Naturally \
  Math::Random \
  Spreadsheet::ParseExcel \
  IO::String \
  JSON \
  Data::Stag

# Install database connectivity packages
RUN apt-get install --yes \
  libdbi-perl \
  libdbd-mysql-perl \
  libdbd-pg-perl \
  libdbd-sqlite3-perl

RUN cpanm \
  DB_File

# Install GD and other graphics dependencies
RUN apt-get install --yes \
 libgd2-xpm-dev

RUN cpanm \
  GD \
  SVG \
  SVG::Graph
RUN cpanm \
  Bio::Phylo

RUN cpanm -v \
  https://github.com/bioperl/bioperl-live/archive/master.tar.gz

RUN cpanm \
  Bio::ASN1::EntrezGene

# Install JDK
RUN apt-get install -yqq python-software-properties software-properties-common
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get -y update
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
RUN echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
RUN apt-get -y install oracle-java8-installer

RUN mkdir /qomo
VOLUME /qomo

RUN mkdir /opt/mapr
VOLUME /opt/mapr

RUN groupadd -g 501 qomo
RUN useradd -u 501 -g qomo qomo

RUN apt-get -yqq install iputils-ping telnet dnsutils

ENV PATH=$PATH:/opt/mapr/hadoop/hadoop-2.7.0/bin/:/opt/mapr/spark/spark-1.6.1/bin/:/mapr/qomo.cbb/qomo_production/bin:/mapr/qomo.cbb/qomo_production/opt/sratoolkit.2.7.0-ubuntu64/bin/

VOLUME /home/qomo/script.sh

USER qomo
ENTRYPOINT /bin/bash /home/qomo/script.sh
