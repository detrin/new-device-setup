# new-device-setup

## MacOS

Customize modifier keys: Settings -> Customize modifier keys -> Switch control and command

## Apps without brew

[oh-my-zsh](https://ohmyz.sh/#install)
[vivaldi](https://vivaldi.com/download/)

## Brew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install pipx
```

### Rosetta

```
# Activate rosetta
/usr/bin/arch -arch x86_64 /bin/zsh
# test if current terminal is using rosetta
sysctl -n sysctl.proc_translated
```
Alternativelty, go to Apps -> Terminal -> Info -> Launch new terminal with Rosetta.
