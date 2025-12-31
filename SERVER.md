# Server setup

If you need to reinstall the server and you locally have to refresh the known hosts
```
ssh-keygen -R <IP>
```

set hostname
```
hostnamectl set-hostname <server-name>
```

## Ubuntu chore packages
```
apt install -y nmap
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

## oh-my-tmux
On linux usually tmux is installed by default, however, we can do a little bit better with oh-my-tmux https://github.com/gpakosz/.tmux
```
cd
git clone --single-branch https://github.com/gpakosz/.tmux.git
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .
```

## motd

https://manytools.org/hacker-tools/ascii-banner/ and use ANSI Shadow
```
rm /etc/motd
nano /etc/motd
```

## ssh
```
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDPYNTPp+pKUIEMtU+Gb+iUF6742+IOTfQH8epkGH3ztuEPHbsQpQfu+x87pt7hzHlRuii5a4m8aXRsvSoLMTZQZ83IS7xVFu5HU0XMjazcbjqySw5kP3gMEtuMqCFaoMv2pDbxATYLK4ygXNaPZH2AzYnWZ53z6HbnsloRsNUlFbH8C3oIGw1ePiCZxRK4QhvPgVOtPLEgwfSPyMWB4ezEo91pyG57u1XkuDp0yBVo6KfLuQxc0oh1GARODomeZ7X8hyyIfEeCQ2mOmrS2l54Go+A6bg035b6j1552M5Qrhbs1Dvmuam5dXh+imUc+a+i7p/MHFtdimFJ1XFms1FfD hermanda@Daniels-Laptop.lan' >> .ssh/authorized_keys
```
Disable the password login
```
nano /etc/ssh/sshd_config
```
change then the following rows
```
PasswordAuthentication no
UsePAM no
```
possibly you will need to delete the following on Ubuntu
```
rm /etc/ssh/sshd_config.d/50-cloud-init.conf
```
then restart the sshd service
```
systemctl daemon-reload
systemctl restart sshd
# maybe you will need to run also 
systemctl restart ssh
```

## fail2ban
```
apt install fail2ban
systemctl enable fail2ban
systemctl start fail2ban 
```

## tailscale
It is really convenient see https://tailscale.com/kb/1031/install-linux and https://tailscale.com/kb/1193/tailscale-ssh
```
curl -fsSL https://tailscale.com/install.sh | sh; tailscale up
```
authenticate via google then run on servers
```
tailscale up --advertise-tags=tag:servers --ssh
```
to enable the ssh login via tailscale.

## Coolify

Install Coolify
```
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```
Go to Settings -> General -> Domain and set the domain. Ten create DNAS A record that will point to this IP with the domain you want to use (or subdomain). Then go to Servers -> localhost -> start proxy.

## ufw
```
ufw default deny incoming
ufw default allow outgoing

ufw allow 22
ufw allow 443

ufw enable
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
