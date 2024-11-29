# Laravel-Sail-Google-Cloud-servera-uzst-d-ana-un-palai-ana


# Ievads
Šī ir instrukcija kā palaist projektu ar Google Cloud VM (Virtual Machine) instances palaišanu,
tiks istāstīts kā uzstādīt VM insanci, nepepieciešamo rīku uzstādīšana un projekta palaišana tīklā.


# 1. Google Cloud

1. Uztaisi google kontu
2. Ieej [Google Cloud](https://cloud.google.com/)
3. Spied **Get started for free**
4. Turpini Google Cloud konta uzstādīšanu
5. Pēc uzstādīšanas aizej uz meklēšanas rīku un ieraksti **VM instances**
6. Spied **Create instance**

### 1. Servera dzelža Konfigurācija
1.  Galvenie lauki kas ir janokonfigure
	1. Instnaces Vārds
	2. Servera atrašanās Reģions
	3.  General purpose E2 Plāns
	4. e-2 medium Severa jauda 
	5. Boot disk palielināt atmiņu līdz 30gb
	6. Un operatāj sistēma Ubuntu 20.04 LTS
	7. Firewall allow http un https traffic
2. Spied **Create**


### 2. Pieslēdzies pie servera
1. Uzspied uz sava servera 
2. Uzspied uz SSH pogas kas atvērs jaunu cilni pārlūkprogrammā
3. Sāc **serverar rīku konifgurāciju** seko soļiem



### 3. Servera rīku konfigurācija
***SVARĪGI!!!***
**Katru komandu raksti atsevišķi vinu aiz otras**, ja rodas kādas kļūdas pameiģini palaist to pašu komandu velreiz sekojot līdzi kas notiek terminlālā, ja kļūda velprojām paliek tad sāc meklēt internetā kļūdu.
Ja internetā neko neatrodi prasi Māklīgajam intelektam ar savas kļūdas definēšanu

1. Atjaunināt paketes
```
sudo  apt update
```
2. Nepieciešamās komponentes
```
sudo apt install -y ca-certificates  curl   zip unzip vim
```

#### 3. Izstrādes komponentes

<> Node un npm instalēšana
```
sudo apt update
sudo apt install nodejs
sudo apt install npm
```

<> Composer instalēšana
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```
```
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```
```
php composer-setup.php
```
```
php -r "unlink('composer-setup.php');"
```
```
sudo mv composer.phar /usr/local/bin/composer
```

#### 5. PHP instalācija
```
sudo add-apt-repository ppa:ondrej/php
```
``` 
sudo  apt update
```
```
sudo  apt  install -y php8.2 php8.2-cli php8.2-common php8.2-curl php8.2-xml php8.2-zip php8.2-mbstring php8.2-mysql
```

#### 6. Docker instalācija 
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg |  sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"  |  sudo  tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo  apt update
```
```
sudo  apt  install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
```
sudo  apt  install -y docker-compose
sudo  usermod -aG docker  $USER
newgrp docker
```
```
sudo systemctl enable  docker sudo systemctl start docker
```
#### 7. Pārbaudi vai viss ir uz vietas
```
node --version
```
```
php --version
```
```
docker --version
```
```
docker-compose --version
```
```
sudo systemctl status docker
```
``` 
sudo systemctl status chrony`
```



# Projekta palaišana Google Cloud instancē

1. Noklonē savu projektu kas ir bāzēts uz Laravel sail
```
git clone <tavs_repo>
```

2. Ieej savā projektā
```
cd <tavs_projekts>
```

3. Atsvaidzini projekta mainīgās paketes
```
composer update 
composer insall
```
```
npm update
npm i
```

4. Ieveito savu .env failu projektā 

Izveido .env failu
```
touch .env
```

Rediģē failu
```
nano .env
```

Ieliec sava lokālā projekta .env faila saturu serverī
```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_TIMEZONE=UTC
APP_URL=http://localhost

APP_LOCALE=en
APP_FALLBACK_LOCALE=en
APP_FAKER_LOCALE=en_US

APP_MAINTENANCE_DRIVER=file
# APP_MAINTENANCE_STORE=database

PHP_CLI_SERVER_WORKERS=4

BCRYPT_ROUNDS=12

LOG_CHANNEL=stack
LOG_STACK=single
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=db
DB_USERNAME=root
DB_PASSWORD=sail

SESSION_DRIVER=database
SESSION_LIFETIME=120
SESSION_ENCRYPT=false
SESSION_PATH=/
SESSION_DOMAIN=null


BROADCAST_CONNECTION=log
FILESYSTEM_DISK=local
QUEUE_CONNECTION=database

CACHE_STORE=database
CACHE_PREFIX=

MEMCACHED_HOST=127.0.0.1

REDIS_CLIENT=phpredis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379 

MAIL_MAILER=log
MAIL_HOST=127.0.0.1
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

VITE_APP_NAME="${APP_NAME}"
```

Pamaini šos laukus
```
APP_NAME=Tava_projekta_nosaukums
APP_ENV=production
APP_DEBUG=true # pectam kad viss strādās  pārlikis uz false
#APP_DEBUG=false
APP_URL=http://tavas_instances publiskā ip adresse
```


Palaid projektu
```
./vendor/bin/sail up -d
```
Palaid šis komanas lai projekts nokonfigurējas
```
./vendor/bin/sail artisan key:generate
./vendor/bin/sail artisan migrate
./vendor/bin/sail artosan stroage:link
```

Palaid šo komandu lai uzbūvētu projektu
```
npm run build
```



## Tā nu tavs projekts ir palaists
Tagad tu zini kā uzstādīt projektu lietojot Laravel Sail un Linux Ubuntu Servera uzstādīšanas procesu
Veiksmi nākamos darbos :)


