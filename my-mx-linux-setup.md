# 🚀 MX Linux 25 Web Development Setup Guide

### (Ubuntu → Debian-Based Transition)

---

# 🧱 1. System Setup (Prerequisites)

## 🔄 Update Your System

```sh
sudo apt update && sudo apt upgrade -y
```

👉 Same as Ubuntu — because MX Linux is Debian-based

---

## 📦 Install Supporting Libraries

```sh
sudo apt install -y software-properties-common python3-software-properties unzip tree curl libcurl4-openssl-dev libexpat1-dev gettext zlib1g-dev libssl-dev cmake gcc build-essential network-manager libnss3-tools jq xsel sed htop preload vlc
```

### 💡 Notes

- Unlike Manjaro, Debian still uses `-dev` packages
- `build-essential` remains the same (no `base-devel` switch)

---

## 🎬 Install Codecs

```sh
sudo apt install -y gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav ffmpeg
```

---

## 🔤 Install Fonts

```sh
sudo apt install -y fonts-firacode
```

---

## 🖥️ Install Ghostty (Alternative Approach)

⚠️ Ghostty via PPA is unreliable on Debian-based systems.

### Recommended:

Use Flatpak instead

```sh
sudo apt install flatpak -y
flatpak install flathub com.mitchellh.ghostty
```

---

## 🔥 Firewall Setup

```sh
sudo apt install ufw -y
sudo systemctl enable ufw
sudo systemctl start ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw enable
```

---

## 🧹 Cleanup

```sh
sudo apt autoremove -y
```

---

# 🛠️ 2. Development Essentials

## 🌐 Install Curl

```sh
sudo apt install curl -y
```

---

## 🔧 Install Git

```sh
sudo apt install git -y
```

### Configure Git

```sh
git config --global user.name "your-name"
git config --global user.email "your@email.com"
```

---

## 💻 VS Code Font Setup

```json
"editor.fontFamily": "Fira Code",
"editor.fontLigatures": true
```

---

# 🐚 ZSH & Oh My Zsh

## Install ZSH

```sh
sudo apt install zsh -y
```

## Make Default

```sh
chsh -s $(which zsh)
```

👉 Log out and back in

---

## Install Oh My Zsh

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## Install ZSH Autosuggestions

```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

---

## Enable PATH

```sh
sed -i 's|# export PATH=$HOME/bin:$HOME/.local/bin:/usr/local/bin:$PATH|export PATH=$HOME/bin:$HOME/.local/bin:/usr/local/bin:$PATH|g' ~/.zshrc
```

---

## Enable Plugin

```sh
sed -i 's|plugins=(git)|plugins=(git zsh-autosuggestions)|g' ~/.zshrc
source ~/.zshrc
```

---

## 🧩 Install MY ZISHH

```sh
git clone https://github.com/FortunateSilas/my.git
```

```sh
echo "\n# Zish\nsource ${HOME}/my/my" >> ~/.zshrc && source ~/.zshrc
```

---

## ⚡ Install Powerlevel10k

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
```

```sh
echo "source ~/powerlevel10k/powerlevel10k.zsh-theme" >> ~/.zshrc && exec zsh
```

---

## Configure

```sh
p10k configure
```

---

# ⚡ 3. Volta (Node.js Manager)

## Install Volta

```sh
curl https://get.volta.sh | bash
```

```sh
source ~/.zshrc
```

---

## Install Node

```sh
volta install node
```

---

## Install Global Packages

```sh
npm install -g yarn tldr trash shelljs ndb gulp grub webpack electron express async validator mkdirp
```

---

# 🐘 4. PHP & Composer (Debian Approach)

## ⚠️ Important Difference from Ubuntu

We **avoid PPAs** like:

```sh
add-apt-repository ppa:ondrej/php
```

---

## Install PHP (Default Debian Version)

```sh
sudo apt install -y php php-curl php-mbstring php-xml php-zip php-mysql php-bcmath php-cli php-common
```

---

## Install Composer

```sh
sudo apt install composer -y
```

---

## Add Composer to PATH

```sh
echo 'export PATH="$HOME/.config/composer/vendor/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

---

# 🌐 5. Valet Linux & MariaDB

## Remove Apache (if installed)

```sh
sudo apt remove apache2 -y
```

---

## Install MariaDB

```sh
sudo apt install mariadb-server -y
```

---

## Secure & Initialize

```sh
sudo mysql_secure_installation
```

---

## Start & Enable

```sh
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

---

## Install Valet

```sh
composer global require genesisweb/valet-linux-plus
```

---

## Install Valet Services

```sh
valet install --mariadb
```

---

## Set Domain

```sh
valet domain test
```

---

# 🛢️ MySQL / MariaDB Root Setup

## Login

```sh
sudo mysql -uroot
```

## Run Commands

```sql
use mysql;
SELECT User, Host FROM mysql.user;
DROP USER 'root'@'localhost';
CREATE USER 'root'@'localhost' IDENTIFIED BY '';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

---

## Exit

```sh
\q
```

---

## Test

```sh
mysql -uroot
```

---

# 🌍 6. WordPress & Laravel

## 🧰 Install WP-CLI

```sh
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
```

```sh
chmod +x wp-cli.phar
```

```sh
sudo mv wp-cli.phar /usr/local/bin/wp
```

```sh
wp --info
```

---

## 🚀 Install Laravel Installer

```sh
composer global require "laravel/installer"
```

---

## Ensure PATH

```sh
echo 'export PATH="$HOME/.config/composer/vendor/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

---

# 🧹 Final Cleanup

```sh
sudo apt autoremove -y
```

---

# 🎯 Final Setup Overview

You now have:

✅ ZSH + Oh My Zsh + Powerlevel10k
✅ Node (Volta-managed)
✅ PHP (Debian stable)
✅ Composer
✅ Valet Linux
✅ MariaDB
✅ WP-CLI
✅ Laravel Installer

---

# 🔁 Ubuntu → MX Linux Key Differences Recap

| Feature           | Ubuntu      | MX Linux (Debian) |
| ----------------- | ----------- | ----------------- |
| PPAs              | Common      | Avoid             |
| PHP versions      | Flexible    | Stable            |
| Package freshness | Newer       | Older but stable  |
| Snap              | Default     | Not used          |
| Philosophy        | Convenience | Stability         |

---

# 💬 If you want next

I can:

- Turn this into a **PDF like the Manjaro one**
- Create a **one-command installer script**
- Or optimize specifically for your **WordPress + Bricks + Valet workflow**

Just tell me 👍
