1. openstack에 instance 생성.
* project : cdnetworks. portal_team
* instance : test_postman (OS : ubuntu)
2. test서버에 script를 통하여 설치
```
#!/bin/bash

echo "----------------------------"
echo "gitlab runner Installed"
echo "----------------------------\n"

export http_proxy="http://labproxy.cdnetworks.com:8000"
export https_proxy="http://labproxy.cdnetworks.com:8000"
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | bash
apt-get install -y gitlab-runner

echo "gitlab-runner ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
echo "-----------"
echo "git rebuild"
echo "-----------\n"
apt-get install -y build-essential fakeroot dpkg-dev
mkdir ~/git-openssl
cd ~/git-openssl
apt-get source git
apt-get build-dep -y git
apt-get install -y libcurl4-openssl-dev
dpkg-source -x git_1.9.1-1ubuntu0.8.dsc
cd git-1.9.1
sed -i 's/libcurl4-gnutls-dev/libcurl4-openssl-dev/g' debian/control
dpkg-buildpackage -rfakeroot -b
dpkg -i ../git_1.9.1-1ubuntu0.8_amd64.deb
  
echo "----------------------------------"
echo "git clone spectrum-api & settings "
echo "--------------------------------\n"
cd /root
git config --global http.proxy http://labproxy.cdnetworks.com:8000
git clone https://git.cdnetworks.com/hwaeun.seo/spectrum-api.git
cd spectrum-api/misc
apt-get install -y python-pip
apt-get install -y python-dev
apt-get install -y python-mysqldb
apt-get install -y libmysqlcppconn-dev
apt-get install -y libsasl2-dev
apt-get install -y freetds-dev
pip install requests --proxy http://labproxy.cdnetworks.com:8000 -r pip_requirements.txt
pip install requests --proxy http://labproxy.cdnetworks.com:8000 memcache
mkdir -p /usr/local/cdnet/logs/spectrum_api && touch /usr/local/cdnet/logs/spectrum_api.log
echo "101.79.230.85   ngpdb.cdnetworks.com statdb.cdnetworks.com cdb.cdnetworks.com" >> /etc/hosts

echo "==============="
echo "newman install "
echo "===============\n"

curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
apt-get install -y nodejs
echo "strict-ssl = false" > .npmrc
https_proxy='http://labproxy.cdnetworks.com:8000' npm install -g npm
https_proxy='http://labproxy.cdnetworks.com:8000' npm install newman -g
   
echo "-------------------------"
echo "make upstart spectrum-api"
echo "-------------------------\n"
echo "
start on runlevel [2345]
stop on runlevel [016]
respawn
chdir /root/spectrum_api
exec python manage.py runserver 0.0.0.0:8001" > /etc/init/spectrum-api.conf
start spectrum-api
```

3. test서버에 runner 설정
* http://allroundplaying.tistory.com/21




