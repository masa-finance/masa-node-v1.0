---
## **Обратите внимание, код разбит на блоки умышленно, блок состоящий из нескольких строк можно копировать и запускать целиком. (используя кнопку копирования в правом верхрем углу блока)**
## Установка происходит для пользователя masa, который создается в процессе. Этот пользователь является безопасным ко взлому, у него нет прав sudo, нет пароля, нет терминала по умолчанию и нет прав подключения извне.
## Команды установки могут выполняться под любым начальным пользователем (как root, так и обычным ), в процессе предусмотрены повышения привелегий которые в случае обычного пользователя будут запрашивать его пароль.
---

# Установка Masa node ("Вариант установки как сервис") актуально для Release v1.03-testnet.2.0

_базовые требования к серверу
2 CPU/4 GB RAM/80 GB HDD_

## Устанавливаем ubuntu 20.04 LTS Server (в минимальном текстовом режиме)


### Обновляем Ubuntu
```
sudo apt-get update && sudo apt-get upgrade -y
```

## Тут опционально следует тюнинг системы (вынесен ниже по тексту)


### Устанавливаем полезные пакеты 
```
sudo apt install apt-transport-https net-tools git mc sysstat atop curl tar wget clang pkg-config libssl-dev jq build-essential make ncdu -y
```

### Создадим пользователя masa
```
sudo addgroup p2p 
sudo adduser masa --ingroup p2p --disabled-password --disabled-login --shell /usr/sbin/nologin --gecos ""
```

### Install GO 1.17.5
```
ver="1.17.5"
cd ~
wget --inet4-only "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> ~/.profile
source ~/.profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> /home/masa/.profile
```

### Собираем бинарные пакеты ноды
```
sudo su masa -s /bin/bash
```

```
cd ~
source ~/.profile
git clone https://github.com/masa-finance/masa-node-v1.0
cd masa-node-v1.0/src
make all
exit
```

### устанавливаем бинарные пакеты в систему
```
sudo -i 
```

```
cp /home/masa/masa-node-v1.0/src/build/bin/* /usr/local/bin
exit
```

### Инициализация ноды
```
sudo su masa -s /bin/bash
```

```
cd ~
source ~/.profile
cd $HOME/masa-node-v1.0
geth --datadir data init ./network/testnet/genesis.json
exit
```

### Cоздаем сервис (сменить NODE_NAME на уникальное, не использовать пробел < > |)
```
sudo -i
```

```
NODE_NAME="Измени-имя_ноды"
```

```
tee /etc/systemd/system/masad.service > /dev/null <<EOF
[Unit]
Description=MASA
After=network.target
[Service]
Type=simple
User=masa
ExecStart=/usr/local/bin/geth \\
--identity ${NODE_NAME} \\
--datadir /home/masa/masa-node-v1.0/data \\
--bootnodes enode://91a3c3d5e76b0acf05d9abddee959f1bcbc7c91537d2629288a9edd7a3df90acaa46ffba0e0e5d49a20598e0960ac458d76eb8fa92a1d64938c0a3a3d60f8be4@54.158.188.182:21000,enode://c4430473f2774e17f7a83b389bdb6c6055e9de21263f014be4c88f0a117fa13e6cd14cce22abd0ebbe366863dea1687fa2eac45be7c25075db0f06dd42c27478@45.56.101.144:30303 \\
--emitcheckpoints \\
--istanbul.blockperiod 10 \\
--mine \\
--miner.threads 1 \\
--syncmode full \\
--verbosity 5 \\
--networkid 190260 \\
--rpc \\
--rpccorsdomain "*" \\
--rpcvhosts "*" \\
--rpcaddr 127.0.0.1 \\
--rpcport 8545 \\
--rpcapi admin,db,eth,debug,miner,net,shh,txpool,personal,web3,quorum,istanbul \\
--port 30300
Restart=on-failure
RestartSec=10
LimitNOFILE=4096
Environment="PRIVATE_CONFIG=ignore"
[Install]
WantedBy=multi-user.target
EOF
exit
```

### Запуск сервиса, включение автозагрузки и проверка статуса 
```
sudo systemctl daemon-reload
sudo systemctl enable masad
sudo systemctl restart masad
```

```
sudo systemctl status masad
```

## Готово, нода установлена

---

# Работа с нодой

### Проверка логов (обычно не нужна, просто для примера - можно пропустить)
вариант 1 - полный лог 
```
journalctl -u masad -f
```
вариант 2 - лог с фильтром 
```
journalctl -u masad -f |grep "new block"
```
Выход из просмотра логов по Ctrl+c

### Так запускается консоль управления нодой (geth консоль)
```
geth attach ipc:/home/masa/masa-node-v1.0/data/geth.ipc
```

## примеры комманд geth консоли

Каталог ноды (там и вся цепочка хранится и конфиги ноды с ключами и кошельки )
```
admin.datadir
```

Проверка подключения к сети (верный ответ true)
```
net.listening
```

Проверка числа активных подключений (верный ответ больше нуля)
```
net.peerCount
```

Проверка нахождения в состоянии синхронизации (верный ответ false, но он бывает как в полном начале когда еще ничего не скачано, так и в случае полной синхронизации с сетью).
В процессе выдает увеличивающиеся значения первой строки до совпадения со второй.
```
eth.syncing
```

Проверка общего состояния ноды (Смотрим на строку  difficulty: , она должна быть больше еденицы и равна текущему блоку. текущий блок можно спросить у коллег)
```
admin.nodeInfo
```

Полный перечень всех подключений (короткий список)
```
admin.peers.forEach(function(value){console.log(value.network.remoteAddress+"\t"+value.name)})
```

Полный перечень всех подключений (длинный список)
```
admin.peers
```
## Update Masa Node to Release v1.03-testnet.2.0

```
systemctl stop masad
sudo su masa -s /bin/bash
```

### удалим прежнюю цепочку монеты и ранее собранные бинарные пакеты затем обновим репозитарий
```
. ~/.profile
cd ~/masa-node-v1.0
find data/geth/* -type f -not -name 'nodekey' -delete
rm -f src/build/bin/*
git pull
```
### собираем бинарные пакеты
```
cd ~/masa-node-v1.0/src
make all
exit
```

### устанавливаем бинарные пакеты в систему

```
sudo -i 
```

```
cp -f /home/masa/masa-node-v1.0/src/build/bin/* /usr/local/bin
exit
```

### Инициализация ноды

```
sudo su masa -s /bin/bash
```

```
cd ~
source ~/.profile
cd $HOME/masa-node-v1.0
geth --datadir data init ./network/testnet/genesis.json
exit
```


### Обновим файл сервиса (сменить NODE_NAME на уникальное, не использовать пробел < > |)

```
sudo -i
```

```
NODE_NAME="Измени-имя_ноды"
```

```
tee /etc/systemd/system/masad.service > /dev/null <<EOF
[Unit]
Description=MASA
After=network.target
[Service]
Type=simple
User=masa
ExecStart=/usr/local/bin/geth \\
--identity ${NODE_NAME} \\
--datadir /home/masa/masa-node-v1.0/data \\
--bootnodes enode://91a3c3d5e76b0acf05d9abddee959f1bcbc7c91537d2629288a9edd7a3df90acaa46ffba0e0e5d49a20598e0960ac458d76eb8fa92a1d64938c0a3a3d60f8be4@54.158.188.182:21000,enode://c4430473f2774e17f7a83b389bdb6c6055e9de21263f014be4c88f0a117fa13e6cd14cce22abd0ebbe366863dea1687fa2eac45be7c25075db0f06dd42c27478@45.56.101.144:30303 \\
--emitcheckpoints \\
--istanbul.blockperiod 10 \\
--mine \\
--miner.threads 1 \\
--syncmode full \\
--verbosity 5 \\
--networkid 190260 \\
--rpc \\
--rpccorsdomain "*" \\
--rpcvhosts "*" \\
--rpcaddr 127.0.0.1 \\
--rpcport 8545 \\
--rpcapi admin,db,eth,debug,miner,net,shh,txpool,personal,web3,quorum,istanbul \\
--port 30300
Restart=on-failure
RestartSec=10
LimitNOFILE=4096
Environment="PRIVATE_CONFIG=ignore"
[Install]
WantedBy=multi-user.target
EOF
exit
```

### Запуск сервиса, включение автозагрузки и проверка статуса

```
sudo systemctl daemon-reload
sudo systemctl enable masad
sudo systemctl restart masad
sudo systemctl status masad
```

## Готово, нода обновлена

# Опциональный тюнинг системы
### Удаляем снапы из системы (опционально)
```
snap list
snap remove lxd
snap remove core18
snap remove snapd
apt purge snapd
```

```
rm -rf ~/snap
rm -rf /home/chia/snap
rm -rf /var/snap
rm -rf /var/lib/snapd
```

### Удалить привязку к облаку (опционально)
```
sudo touch /etc/cloud/cloud-init.disabled
```

  # выключить все флажки кроме последноего "None"
```
dpkg-reconfigure cloud-init
```

```
sudo apt-get purge cloud-init
```

```
sudo rm -rf /etc/cloud/ && sudo rm -rf /var/lib/cloud/
```

Тут внимательно - будет перезагрузка (отвязка от облачных сервисов требует перезагрузки)
```
sudo reboot
```

### Выключить автоапгрейд (опционально)
```
sudo systemctl mask unattended-upgrades.service
sudo systemctl stop unattended-upgrades.service
```

### Статус строка  screen (опционально)
```
echo "hardstatus alwayslastline
hardstatus string '%{gk}[ %{G}%H %{g}][%{= kw}%-w%{= BW}%n %t%{-}%+w\][%= %{=b kR}(%{W} %h%?(%u)%?%{=b kR} )%{= kw}%=][%{Y}%l%{g}]%{=b C}[ %d.%m.%Y %c:%s ]%{W}'
defscrollback 10000
"  > ~/.screenrc
```

### Подключаем синхронизацию времени
`timedatectl list-timezones` # тут смотрим вывод и выбираем свою зону
`timedatectl set-timezone Europe/Moscow` # задаем выбранную

```
apt-get install ntp
sntp --version
```
>sntp 4.2.8p12@1.3728-o (1)

```
nano /etc/ntp.conf
```

произвести вот такие правки
```
# pool 0.ubuntu.pool.ntp.org iburst
# pool 1.ubuntu.pool.ntp.org iburst
# pool 2.ubuntu.pool.ntp.org iburst
# pool 3.ubuntu.pool.ntp.org iburst
server 0.ru.pool.ntp.org
server 1.ru.pool.ntp.org
server 2.ru.pool.ntp.org
server 3.ru.pool.ntp.org
```

```
service ntp restart
service ntp status
systemctl enable ntp
ntpq -p
```
