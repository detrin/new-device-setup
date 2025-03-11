# new-device-setup

## MacOS

## Apps without brew

[oh-my-zsh](https://ohmyz.sh/#install)

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
