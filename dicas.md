# Dicas

- [Pacotes](#pacotes)

---

# Pacotes

## Base apt
Lista a informacao de todos os pacotes instalados pelo apt

     vim /var/lib/dpkg/status

## Base total
Diretorio com a lista da base total do debian, é montada pelo comando apt update. O que está nela é o que se tem na nuvem

    cd /var/lib/apt/lists/

## lista dos repositorios
Repositorios em que o apt deve buscar por pacotes..

    /etc/apt/sources.list

## lista com pacotes a serem atualizados
    apt list --upgradable

## Pacote download
Nao instala, apenas faz o download

    apt install <packger> -d
    cd /var/cache/apt/archives

## Extrai arquivos do .deb
    dpkg -X <pacager.deb> /tmp 

## Mostra os arquivos instalados do pacote
    dpkg -L <pacager>

## Mostra a integridade dos arquivos do pacote
    debsums -a <pacote>

## Mostra quem foi o pacote q colocou este arquivo ai
    dpkg -S /usr/bin/zstdless

## mostra quem depende deste pacote
    apt rdepends apache2-bin

## apt
    apt policy
    apt policy pacote-name


## Informacao do pacote
    apt show <pacote>

## Pacotes nao debian
Lista quais pacotes instalados n estao nos repositorios debian

    apt list '~i!~Odebian'







## Criar hd virtual
    dd if=oarquivo.iso of=/dev/sdb bs=16M oflag=sync status=progress
    dd if=/dev/zero of=curso.gnu.img bs=1G count=7

## Espaco usado
    df -hT

## Lista block devices
    lsblk -l

## Lista a ordem de boot
    efibootmgr -v

## Mostar msgs de firmware
    journalctl -b -g firm

## hex
    xxd <nameArquivo>

### GRUB

## Carregat grub.cfg
Carrega o arquivo de configuracao responsavel por criar o menu do grub

    grub> configfile /boot/grub/grub.cfg

## carrega kernel e initrd
comando grub para carregar o kernel

    grub> linux /boot/vmlinuz-5.16.0-5-amd64 root=/dev/vda1
    grub> initrd /boot/initrd.img-5.16.0-5-amd64
    grub> boot

logo apos, pronpt initramfs, veremos onde esta a particao com o /

    (initramfs) cat /proc/partitions

mantar o /

    (initramfs) mount -t ext4 -o noatime,modiratime /dev/vda2 /root

sair do script init = (initramfs)

    (initramfs) exit



### ---

## mudar brilho tela

    xrandr -q
    xrandr --output eDP --brightness 0.9

## reler tabela de particao

    partprobe

## locales

    dpkg-reconfigure locales

## local do arquivo
    which update-grub



### kit kretcheu

Particionar pendriver e criar particoes
    fdisk /dev/sda
    /dev/sda1     2048     4095     2048      1M BIOS inicialização   *
    /dev/sda2     4096   208895   204800    100M Sistema EFI
    /dev/sda3   208896 15630302 15421407    7,4G Linux sistema de arquivos
 
criar sistemas de arquivos
    mkfs.vfat /dev/sda2
    mkfs.ext4 /dev/sda3

montar sistemas de arquivos
    mount /dev/sda3/ /debian

copiar arquivos ( debootstrap )
    debootstrap bullseye /debian http://deb.debian.org/debian

chroot
    apt install arch-install-scripts
    arch-chroot /debian

-mudar /etc/apt/source.list
    deb http://deb.debian.org/debian bullseye main contrib non-free
    deb http://deb.debian.org/debian bullseye-updates main contrib non-free
    deb http://security.debian.org/ bullseye-security main contrib non-free
    deb http://deb.debian.org/debian bullseye-backports main contrib non-free

-usuarios
    adduser illi
    passwd (define senha de root )

-instalar pacotes
    apt install linux-image-amd64
    apt install bash-completion locales
    dpkg-reconfigure locales

instalando o grup (bios)
    apt install grub-pc
    grub-install /dev/sda

-preparar para boot( bios)
    update-grub

-preparar para boot (efi)
    mkdir /boot/efi
    mount /dev/sda2 /boot/efi
    apt install grub-efi-amd64-bin
    grub-install --target=x86_64-efi /dev/sda --removable
    update-grub

-verificar resolve.conf
    cat /etc/resolv.conf
    nameserver 8.8.8.8

arquivo reponsavel por montar no inicializacao do kernel
    /etc/fstab

sair do chroot
    genfstab -U /debian/ > /debian/etc/fstab
    vim /debian/etc/fstab

finalizar
    umount /debian/boot/efi
    umount /debian/

## descobir local/caminho do arquivo
    which <modulo>




### modulos

# local dos modulos do kernel
    /lib/modules/<kernel>

# descobrir versao do kernel
    uname -a

# codigo fonte souce linux
    /urs/src/linux-source-5.16/

# Configuração de Múltiplas Redes Wi-Fi com `wpa_supplicant`

## 1. Criar/editar o arquivo de configuração

Edite o arquivo do `wpa_supplicant`:

```sh
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Adicione suas redes Wi-Fi:

```ini
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=BR

network={
    ssid="Rede1"
    psk="SenhaDaRede1"
    priority=5
}

network={
    ssid="Rede2"
    psk="SenhaDaRede2"
    priority=10
}

network={
    ssid="Rede3"
    psk="SenhaDaRede3"
    priority=7
}
```

- ``: redes com maior prioridade são preferidas.
- Para Wi-Fi **sem senha**, use:
  ```ini
  network={ ssid="WiFi_Aberto" key_mgmt=NONE }
  ```

---

## 2. Configurar para iniciar automaticamente

Edite `/etc/network/interfaces`:

```sh
sudo nano /etc/network/interfaces
```

Adicione:

```ini
auto wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

Reinicie a rede:

```sh
sudo systemctl restart networking
```

---

## 3. Testar a conexão manualmente

```sh
sudo wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
sudo dhclient wlan0
ip a show wlan0
```

---

## 4. Ativar `wpa_supplicant` no boot

```sh
sudo systemctl enable wpa_supplicant
sudo systemctl restart wpa_supplicant
```

---

## 5. Verificar redes disponíveis e status

```sh
sudo wpa_cli scan
sudo wpa_cli scan_results
sudo wpa_cli status
```

---

Agora, o Debian se conectará automaticamente à melhor rede disponível!




