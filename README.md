# Server Setup

List of todos to setup a new Debian/Ubuntu based server for production and 
development environment.

## 🏁 First Thing First

```console
$ sudo apt update -y
```

## ⚒ The Utilities

Install basic utilities

```console
$ sudo apt install -y tmux htop vim net-tools zsh zip unzip scp
```


### Install oh-my-zsh

```console
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

**Add some plugin to make life easier**

Install `zsh-autosuggestions`:

```console
$ git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

Install `zsh-syntax-highlighting`:

```console
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

Edit configuration file:

```console
$ vim ~/.zshrc
```

Append `zsh-autosuggestions` & `zsh-syntax-highlighting` to `plugins()` section:

```ini
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

Save quit and restart `zsh`:

```console
$ source ~/.zshrc
```

### Install Tmux Plugin Manager

```console
$ git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```


### Some keybindings and plugins for tmux

Create new configuration file:

```console
$ vim ~/.tmux.conf
```

And put code bellow:

```ini
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
```console
$ Ctrl + A => I
# or restart the session/terminal emulator
```




## 🚦The Web Server

```console
$ sudo apt remove apache2
$ sudo apt install nginx
```


## 🕹The Programming Languages

### Basic PHP Installation for Laravel Project

Install PHP and required extentions:
```console
$ sudo add-apt-repository ppa:ondrej/php
$ sudo apt-get update
$ sudo apt-get install -y php7.3 \ 
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

```console
$ cd ~
$ curl -sS https://getcomposer.org/installer -o composer-setup.php
```

Verify composer  SHA-384 hash [from here](https://composer.github.io/pubkeys.html)

```console
$ HASH={put_the_hash_here}
```

Make sure the installer is valid:
```console
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified';  } else { echo 'Installer corrupt'; unlink('composer-setup.php');  } echo PHP_EOL;"
```

Finish the installation:
```console
$ sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```




### NodeJS

Install latest version of NodeJS
```console
$ curl -sL https://deb.nodesource.com/setup_current.x | sudo -E console -
$ sudo apt-get install -y nodejs
```

## 💾 The Databases

```console
$ sudo apt install mysql-server redis-server
```

### Enable Remote Access For MySQL

> ⚠ Allow remote access is not recommended for production environment, 
but if you insist, here how to do it:

Make sure 3306 port not blocked by firewall:

```console
$ iptables -A INPUT -i eth0 -p tcp --destination-port 3306 -j ACCEPT
$ service iptables save
$ service iptables restart
```

Depends on your mysql.conf, find line with `bind-address` and change it like so:

```ini
bind-address = 0.0.0.0
```

Enable user remote access privileges:

```sql
mysql> CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'yourpassword';

mysql> GRANT ALL PRIVILIGES ON *.* TO 'root'@'%';

mysql> FLUSH PRIVILEGES;
```

And finally, restart mysql services:

```console
$ sudo service mysql restart
```


## 🐳 The Docker

Install Docker:

```console
$ sudo apt-get install \
	apt-transport-https \
	ca-certificates \
	curl \
	gnupg-agent \
	software-properties-common

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo apt update && sudo apt-get install docker-ce docker-ce-cli containerd.io
```

After installation:

```console
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ sudo reboot
```

After reboot:
```console
$ newgrp docker 
```


