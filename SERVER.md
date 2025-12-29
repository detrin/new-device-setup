# Server setup

If you need to reinstall the server and you locally have to refresh the known hosts
```
ssh-keygen -R <IP>
```

set hostname
```
hostnamectl set-hostname <server-name>
```

## zsh

Install zsh
```shell
apt install -y zsh
chsh -s $(which zsh)
```

Install Oh my zsh https://ohmyz.sh/#install
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

[https://github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#oh-my-zsh)
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"
```

Open ~/.zshrc, find the line that sets ZSH_THEME, and change its value to `powerlevel10k/powerlevel10k`.

## motd

https://manytools.org/hacker-tools/ascii-banner/ and use ANSI Shadow
```
rm /etc/motd
nano /etc/motd
```

Copy ssh public key
```
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDPYNTPp+pKUIEMtU+Gb+iUF6742+IOTfQH8epkGH3ztuEPHbsQpQfu+x87pt7hzHlRuii5a4m8aXRsvSoLMTZQZ83IS7xVFu5HU0XMjazcbjqySw5kP3gMEtuMqCFaoMv2pDbxATYLK4ygXNaPZH2AzYnWZ53z6HbnsloRsNUlFbH8C3oIGw1ePiCZxRK4QhvPgVOtPLEgwfSPyMWB4ezEo91pyG57u1XkuDp0yBVo6KfLuQxc0oh1GARODomeZ7X8hyyIfEeCQ2mOmrS2l54Go+A6bg035b6j1552M5Qrhbs1Dvmuam5dXh+imUc+a+i7p/MHFtdimFJ1XFms1FfD hermanda@Daniels-Laptop.lan' >> .ssh/authorized_keys
```

## Coolify

Install Coolify
```
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```
Go to Settings -> General -> Domain and set the domain. Ten create DNAS A record that will point to this IP with the domain you want to use (or subdomain). Then go to Servers -> localhost -> start proxy.

## ufw
```
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow 22
sudo ufw allow 443

sudo ufw enable
```

https://github.com/chaifeng/ufw-docker
```
sudo wget -O /usr/local/bin/ufw-docker https://github.com/chaifeng/ufw-docker/raw/master/ufw-docker
sudo chmod +x /usr/local/bin/ufw-docker
```
```
ufw-docker install
systemctl restart ufw
```
If you want to list the docker containers
```
docker container ls
```
```
ufw-docker allow coolify-proxy 443/tcp
```
On contabo tcp/53 is open probably by the MV management tool.

```
crontab -e
# add at the last line
ufw-docker allow coolify-proxy 443/tcp
```
