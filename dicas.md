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



