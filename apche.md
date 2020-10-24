## Docker: Php-apache
- Ini 要設置

  ```bash
  cp ini
  ```

- Mysql 會缺

  ```bash
  docker-php-ext-install mysqli
  docker-php-ext-enable mysqli
  ```
  打開 php.ini
  ```ini
  extension=/usr/local/lib/php/extensions/no-debug-non-zts-20170718/mysqli.so
  ```
  ```bash
  apachectl restart
  ```

- Composer 需要安裝

  ```bash
  php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

  php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

  php composer-setup.php --install-dir=bin

  php -r "unlink('composer-setup.php');"
  ```

- Composer require會缺 zip

  ```bash
  apt install zip unzip
  ```
  若 Apt 找不到 zip unzip
  ```bash
  apt-get update
  Apt-get upgrade
  ```

- 修改 APACHE 預設目錄

  修改 apache2 的預設文件目錄 (預設是在 /var/www)
  修改命令：
  ```bash
  sudo gedit /etc/apache2/sites-enabled/000-default
  ```
