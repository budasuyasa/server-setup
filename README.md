# Server Setup

List of todos to setup a new Debian/Ubuntu based server for production and 
development environment.

## The First Thing First

```
sudo apt update -y
```

## The Utilities

Basic Utilities

```
sudo apt install -y tmux htop vim net-tools zsh zip unzip scp
```


### Install oh-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

**Add some plugin to make life easier**

Install zsh-autosuggestions:
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

Install zsh-syntax-highlighting:

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

Edit configuration file:
```
vim ~/.zshrc
```

Append `zsh-autosuggestions` & `zsh-syntax-highlighting` to `plugins()` section:

```bash
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

Save quit and restart `zsh`:

```bash
source ~/.zshrc
```

### Install Tmux Plugin Manager

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```


### Some keybindings and plugins for tmux

Create new configuration file:

```bash
vim ~/.tmux.conf
```

And put code bellow:

```bash
# remap prefix from C-b to C-a
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

set -g mouse on
set-option -g allow-rename off

# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

# Other examples:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'git@github.com/user/plugin'
# set -g @plugin 'git@bitbucket.com/user/plugin'
set -g @plugin 'tmux-plugins/tmux-yank'
set -g @plugin 'jimeh/tmux-themepack'


# Start windows and panes at 1, not 0
set -g base-index 1
setw -g pane-base-index 1

set -g @themepack 'powerline/block/green'

unbind-key -T copy-mode-vi v

setw -g mode-keys vi
bind-key -T copy-mode-vi 'v' send -X begin-selection     # Begin selection in copy mode.
bind-key -T copy-mode-vi 'C-v' send -X rectangle-toggle  # Begin selection in copy mode.
bind-key -T copy-mode-vi 'y' send -X copy-selection      # Yank selection in copy mode.

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run -b '~/.tmux/plugins/tpm/tpm'

# ！！！importent！！！ 开启24 bit color 其他方式都无效
set -g default-terminal "tmux-256color"
set -ga terminal-overrides ",*256col*:Tc"
set -g mouse on

```

Save, quit and restart tmux:
```bash
Ctrl + A => I
# or restart the session/terminal emulator
```




## The Web Server

```bash
sudo apt install nginx
```


## The Programming Languages

### Basic PHP Installation for Laravel Project

Install PHP and required extentions:
```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install -y php7.3 \ 
	php7.3-curl \
	php7.3-dev \
	php7.3-gd \
	php7.3-mbstring \
	php7.3-zip \
	php7.3-mysql \
	php7.3-xml \
	php7.3-fpm \
	libapache2-mod-php7.3 \
	php7.3-imagick \
	php7.3-recode \
	php7.3-tidy \
	php7.3-xmlrpc \
	php7.3-intl 
```

Install `composer`:

```bash
cd ~
curl -sS https://getcomposer.org/installer -o composer-setup.php
```

Verify composer  SHA-384 hash [from here](https://composer.github.io/pubkeys.html)

```bash
HASH={put_the_hash_here}
```

Make sure the installer is valid:
```bash
php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified';  } else { echo 'Installer corrupt'; unlink('composer-setup.php');  } echo PHP_EOL;"
```

Finish the installation:
```bash
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```




### NodeJS

Install latest version of NodeJS
```bash
curl -sL https://deb.nodesource.com/setup_current.x | sudo -E bash -
sudo apt-get install -y nodejs

```

## The Databases

```bash
sudo apt install mysql-server redis-server
```

### The Docker

Install Docker
```
sudo apt-get install \
	apt-transport-https \
	ca-certificates \
	curl \
	gnupg-agent \
	software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt update && sudo apt-get install docker-ce docker-ce-cli containerd.io
```

After installation:

```
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot
```

After Reboot:
```
newgrp docker 
```


