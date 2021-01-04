# gitea-configuration

Build notes for getea server.
## Server Setup
### Installing pre-requisites

We are building on a ubuntu/focal/cloud (from lxc's images) container with preseeded admin accounts. 

```
apt-get -y install curl postgresql apache2 git
apt-get install postfix
... add as a Satelite (null client) ...
```
We want to use a single git user so we add it (will deal with this later)

```
adduser --system --shell /bin/bash --group --disabled-password --home /home/git git
```
We are going to use the package provided by packaging.gitlab.io 

```
curl -sL -o /etc/apt/trusted.gpg.d/morph027-gitea.asc https://packaging.gitlab.io/gitea/gpg.key
deb [trusted=yes arch=amd64] https://packaging.gitlab.io/gitea gitea main" | sudo tee /etc/apt/sources.list.d/morph027-gitea.list
update.sh
apt-get install gitea
```
### Setting up postgresql database
```
su - postgres
postgres@shelly:~$ createuser -P git
... add passwd
postgres@shelly:~$ createdb gitea -O git
```
### Initial configuration
Once gitea is installed go to myservername:3000 and navigate to the login in the upper right corner. Fill in the database,username, and dbpassword. Replace localhost with your servers fqdn. Create admin user (remember password here)



## references
* [https://gitlab.com/packaging/gitea](https://gitlab.com/packaging/gitea)
* [https://bryangilbert.com/post/devops/how-to-setup-gitea-ubuntu/](https://bryangilbert.com/post/devops/how-to-setup-gitea-ubuntu/) 
* [https://luxagraf.net/src/gitea-nginx-postgresql-ubuntu-1804](https://luxagraf.net/src/gitea-nginx-postgresql-ubuntu-1804)
* [https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token)