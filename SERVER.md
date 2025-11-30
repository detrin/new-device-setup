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

## Coolify

Install Coolify
```
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```

## ufw
```
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443

sudo ufw enable
```
