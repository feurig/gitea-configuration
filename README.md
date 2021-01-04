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

## Testing it out.
The first thing we want to do here is to mirror one of our github repositories (this one)
### Mirroring Github Repositories.
At some point we want to automate mirroring all of our repositories. For now we want to test it out. To do this we create a personal-access-token from our github developer tools. (save the token somewhere as it will not be recoverable). Once we have that token we select New migration. Fill in the https://github.com/myuser/myrepo and paste the token into the form, select mirror and the magic begins.

### Editiing the mirror interval
The default mirror interval is 8 hours with a minimum of 10 minutes. 
To fix this we add the following to /etc/gitea/app.ini

```
nano /etc/gitea.app.ini
...
[cron]
ENABLED = true
RUN_AT_START = true

[cron.update_mirrors]
SCHEDULE = @every 2m

[mirror]
DEFAULT_INTERVAL = 1h
MIN_INTERVAL = 2m
...
service gitea restart
```



## references
* [https://gitlab.com/packaging/gitea](https://gitlab.com/packaging/gitea)
* [https://bryangilbert.com/post/devops/how-to-setup-gitea-ubuntu/](https://bryangilbert.com/post/devops/how-to-setup-gitea-ubuntu/) 
* [https://luxagraf.net/src/gitea-nginx-postgresql-ubuntu-1804](https://luxagraf.net/src/gitea-nginx-postgresql-ubuntu-1804)
* [https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token)
* [https://jpmens.net/2019/04/15/i-mirror-my-github-repositories-to-gitea/](https://jpmens.net/2019/04/15/i-mirror-my-github-repositories-to-gitea/)
* 