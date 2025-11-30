# Server setup

If you need to reinstall the server and you locally have to refresh the known hosts

`ssh-keygen -R <IP>`

set hostname

`hostnamectl set-hostname <server-name>`

## zsh
`sudo apt install -y zsh`

`chsh -s $(which zsh)`

[https://github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#oh-my-zsh)

`git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"`

Open ~/.zshrc, find the line that sets ZSH_THEME, and change its value to `powerlevel10k/powerlevel10k`.


