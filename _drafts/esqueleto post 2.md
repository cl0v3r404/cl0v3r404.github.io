## Comandos

wget -O - [https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install](https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install "https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install") | bash

`swapoff -a`

blkid | grep swap

nano /etc/fstab

comentar linea de swap

fallocate -l 4G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile

si falla: dd if=/dev/zero of=/swapfile bs=1M count=4096

swapon --show
free -h

apt update
apt install zram-tools

nano /etc/default/zramswap

ALGO=zstd
PERCENT=50
PRIORITY=100

systemctl restart zramswap
systemctl enable zramswap

NAME       TYPE       SIZE   USED PRIO
/dev/zram0 partition    4G     0B  100
/swapfile  file         4G     0B   -2

ajustar en fstab: 
/swapfile none swap sw,pri=10 0 0

echo 'vm.swappiness=100' >> /etc/sysctl.conf

## Configuracion basica



## Activacion docker

en sistema omv-extras habilitar `docker repo` y enable backports (si no falla)

Luego en plugins, instalar `openmediavault-compose`

si falla:
- 