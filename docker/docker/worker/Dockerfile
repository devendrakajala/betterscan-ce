FROM python:3.7
RUN apt update && apt install -y libcurl4-nss-dev libssl-dev tree git sudo ssh rubygems python3-pip npm php default-jdk pipenv jo
WORKDIR /
RUN mkdir -p /srv
RUN git clone https://github.com/marcinguy/scanmycode-ce.git /srv/scanmycode
WORKDIR /srv/scanmycode
RUN git pull
WORKDIR /srv/scanmycode
#RUN pip install -r requirements.txt
RUN pipenv install --system
#RUN pip install pylint===1.9.2
#RUN pip install stripe
WORKDIR /
RUN git clone -b checkmate3 https://github.com/marcinguy/checkmate-ce /checkmate
WORKDIR /checkmate
RUN git checkout checkmate3
RUN git pull
RUN python3 setup.py install
WORKDIR /
#RUN pip uninstall -y blitzdb3
#RUN git clone https://github.com/marcinguy/blitzdb3-ce.git /blitzdb3-ce
#WORKDIR /blitzdb3-ce
#RUN git pull
#RUN python3 setup.py install
WORKDIR /
RUN pip install psycopg2 --upgrade
RUN ln -s /srv/scanmycode/quantifiedcode/settings/default.yml /srv/scanmycode/settings.yml
WORKDIR /root
#RUN wget https://golang.org/dl/go1.17.6.linux-amd64.tar.gz
ENV SHELL /bin/bash
RUN curl -LO https://get.golang.org/$(uname)/go_installer
RUN chmod +x go_installer
RUN ./go_installer
RUN rm go_installer
#RUN sudo tar -C /usr/local -xzf go1.17.6.linux-amd64.tar.gz
RUN /root/.go/bin/go install github.com/securego/gosec/v2/cmd/gosec@latest
RUN mkdir /root/bin
RUN cp /root/go/bin/gosec /root/bin/gosec
RUN gem install brakeman
RUN python3 -m pip install bandit
RUN python3 -m pip install semgrep
RUN npm install -g jshint
WORKDIR /root
RUN git clone https://gitlab.com/marcinguy/trufflehog3-oss.git
RUN cd trufflehog3-oss && python3 setup.py install
RUN pip3 install markupsafe==2.0.1
WORKDIR /srv/scanmycode
RUN mkdir /root/confused/
RUN git clone https://gitlab.com/marcinguy/confused-oss.git /root/confused
RUN tree
WORKDIR /root/confused
RUN /root/.go/bin/go mod init confused
RUN /root/.go/bin/go build confused
RUN tree
#RUN cp /srv/scanmycode/analyzers/confused /root/confused/ #source https://gitlab.com/marcinguy/confused-oss
WORKDIR /root
RUN git clone https://github.com/Raz0r/semgrep-smart-contracts.git
RUN git clone https://github.com/marcinguy/smc-semgrep-js.git semgrepjs
RUN wget https://github.com/pmd/pmd/releases/download/pmd_releases%2F6.41.0/pmd-bin-6.41.0.zip
RUN unzip pmd-bin-6.41.0.zip
RUN mkdir /root/phpscan/
RUN cp /srv/scanmycode/analyzers/progpilot_dev20180720-215434.phar /root/phpscan/
RUN cp /srv/scanmycode/analyzers/find_unicode_control2.py /usr/local/bin/
RUN cp -pr /srv/scanmycode/analyzers/custom-semgrep /root
RUN cp /srv/scanmycode/analyzers/*.yml /root/
RUN cp /srv/scanmycode/sshfeature/ssh /usr/local/lib/python3.7/site-packages/checkmate3-0.2.0-py3.7.egg/checkmate/contrib/plugins/git/lib
RUN mkdir -p /usr/local/lib/python3.7/site-packages/checkmate3-0.2.0-py3.7.egg/checkmate/contrib/plugins/javascript/jshint/js
RUN cp /srv/scanmycode/analyzers/json_reporter.js /usr/local/lib/python3.7/site-packages/checkmate3-0.2.0-py3.7.egg/checkmate/contrib/plugins/javascript/jshint/js/
RUN pip3 install email_validator
#RUN pip3 install celery==4.4.6
RUN pip3 install testresources
RUN pip3 install checkov
RUN curl -s https://raw.githubusercontent.com/armosec/kubescape/master/install.sh | /bin/bash
RUN wget https://github.com/insidersec/insider/releases/download/3.0.0/insider_3.0.0_linux_x86_64.tar.gz
RUN tar -xzf insider_3.0.0_linux_x86_64.tar.gz
WORKDIR /root
RUN git clone https://github.com/marcinguy/graudit-ce graudit
RUN git clone https://github.com/marcinguy/text4shell-ce text4shell-ce
RUN pip install yara-scanner
RUN mkdir /root/yara
RUN cp -pr /srv/scanmycode/analyzers/yara/* /root/yara
RUN curl --silent --output /dev/null https://dl.betterscan.io/docker-build-worker.html  
