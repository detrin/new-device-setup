# new-device-setup

## MacOS

- Customize modifier keys: Settings -> Customize modifier keys -> Switch control and command.
- Natural scrolling: Settings -> Natural scrolling -> Turn off.
- Genereate ssh key for ssh/github https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

```
xcode-select --install
```

## Apps without brew

- [oh-my-zsh](https://ohmyz.sh/#install)
- [vivaldi](https://vivaldi.com/download/)
- [chrome](https://www.google.com/chrome/)
- Amphetamine: go to appstore
- [docker](https://www.docker.com/)
- [vscode](https://code.visualstudio.com/download) https://code.visualstudio.com/docs/setup/mac

## Brew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install pipx python@3.12 python@3.12 python@3.11 wget curl htop openblas
```

## Other
```
pipx install poetry
```
```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Rosetta

```
# Activate rosetta
/usr/bin/arch -arch x86_64 /bin/zsh
# test if current terminal is using rosetta
sysctl -n sysctl.proc_translated
```
Alternativelty, go to Apps -> Terminal -> Info -> Launch new terminal with Rosetta.
