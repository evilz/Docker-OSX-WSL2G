# Install OSX on Docker hosted by WSL2G

## Get image

```sh
docker pull sickcodes/docker-osx:auto
```

## Create Container

- mount windows C folder that exists on wsl
- mount /mnt/wslg/.X11-unix
- open ssh on port 50922 on linux
- Set 8 GB of RAM


```sh

docker run -it \
--device /dev/kvm \
-p 50922:10022 \
-e "DISPLAY=${DISPLAY:-:0}" \
-v /mnt/wslg/.X11-unix:/tmp/.X11-unix \
-e GENERATE_UNIQUE=true \
-e AUDIO_DRIVER=pa,server=unix:/tmp/pulseaudio.socket \
-v "/run/user/$(id -u)/pulse/native:/tmp/pulseaudio.socket" \
-e RAM=8 \
-v "/mnt/c:/mnt/c" \
-e EXTRA="-virtfs local,path=/mnt/c,mount_tag=c,security_model=passthrough,id=c" \
sickcodes/docker-osx:auto
```

## Set keyboard command and mouse direction

1. Open up Settings
2. Select Keyboard
3. Press the Modifier Keys button in the bottom right
4. For the "Control" key option, select Command
5. For the "Command" key options select Control

1. Open up Settings
2. Select Mouse
3. uncheck Scoll direction: Natural

## Enable SSH

1. Open up Settings
2. Select Sharing
3. Check Remote Login (for all users)

on Windows redirect port 22 to wsl to REMOTE_PORT => 50922
```powershell
netsh interface portproxy add v4tov4 listenport=22 listenaddress=127.0.0.1 connectport=REMOTE_PORT connectaddress=REMOTE_HOST_OR_IP_ADDRESS
```

## Install App

### Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### install Oh my posh 2

- Get Nerd font MESLO  (MesloLGM NF)
https://github.com/ryanoasis/nerd-fonts/releases/download/v2.1.0/Meslo.zip

```
brew tap jandedobbeleer/oh-my-posh
brew install oh-my-posh
```

Add the following to ~/.zshrc:
```
eval "$(oh-my-posh --init --shell zsh --config '~/.mytheme.omp.json')"
```

Once added, reload your profile for the changes to take effect.
```
source ~/.zshrc
```


- Chrome 
- VS for mac
- Xcode
- 
