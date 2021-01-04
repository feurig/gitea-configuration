# gitea-configuration
Build notes for getea server.


```
apt-get -y install curl postgresql git
apt-get install postfix
adduser --system --shell /bin/bash --group --disabled-password --home /home/git git
curl -sL -o /etc/apt/trusted.gpg.d/morph027-gitea.asc https://packaging.gitlab.io/gitea/gpg.key
deb [trusted=yes arch=amd64] https://packaging.gitlab.io/gitea gitea main" | sudo tee /etc/apt/sources.list.d/morph027-gitea.list
update.sh
apt-get install gitea
su - postgres
postgres@shelly:~$ createuser -P git
... add passwd
postgres@shelly:~$ createdb gitea -O git
ssh-keygen
cat ~/.ssh/id_rsa.pub
cd ~gitea/gitea-repositories/feurig
git clone --mirror git@github.com:feurig/redmine-configuration.git
