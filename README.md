# BlackArch-pre-install
Solucionando fallos que da la distro "BlackArch linux" al momento de instalarla, por primera vez

Dentro de nuestro BlackArch al momento de haberlo instalado de la pagina oficial: [link](https://blackarch.org/) lo peor que podemos hacer es actualizarla
ya que como esta distro tiene tiempo sin uso dara un monton de errores, para eso cree una serie de pasos que posterior se convertiran en un script
para solucionar estos o por lo menos acercarnos el solucionar los fallos

1. Desinstalacion de paquetes

```shell
function pre_install() {

    # Lista de paquetes a eliminar
    packages_to_remove=(
     	"blackarch-webapp"
        "blackarch-fuzzer"
        "blackarch-scanner"
        "blackarch-proxy"
        "blackarch-windows"
        "blackarch-dos"
        "blackarch-disassembler"
        "blackarch-cracker"
        "blackarch-voip"
        "blackarch-exploitation"
        "blackarch-recon"
        "blackarch-spoof"
        "blackarch-forensic"
        "blackarch-crypto"
        "blackarch-backdoor"
        "blackarch-networking"
        "blackarch-misc"
        "blackarch-defensive"
        "blackarch-wireless"
        "blackarch-automation"
        "blackarch-sniffer"
        "blackarch-binary"
        "blackarch-packer"
        "blackarch-reversing"
        "blackarch-mobile"
        "blackarch-malware"
        "blackarch-code-audit"
        "blackarch-social"
        "blackarch-honeypot"
        "blackarch-hardware"
        "blackarch-fingerprint"
        "blackarch-decompiler"
        "blackarch-config"
        "blackarch-debugger"
        "blackarch-firmware"
        "blackarch-bluetooth"
        "blackarch-database"
        "blackarch-automobile"
        "blackarch-nfc"
        "blackarch-tunnel"
        "blackarch-drone"
        "blackarch-unpacker"
        "blackarch-radio"
        "blackarch-keylogger"
        "blackarch-stego"
        "blackarch-anti-forensic"
        "blackarch-ids"
        "blackarch-gpu"
    )

    # Iterar sobre la lista de paquetes e intentar eliminarlos
    for package in "${packages_to_remove[@]}"; do
        if sudo pacman -Rnsc "$package" --noconfirm &> /dev/null; then
            echo "El paquete $package fue desinstalado correctamente."
        else
            echo "El paquete $package no está instalado o no se pudo desinstalar. Saltando..."
        fi
    done
```

2. Solucionando errores de **keyring** y llaves

Esto lo podemos seguir directamente de su pagina [oficial](https://blackarch.org/faq.html), pero para resumirlo podemos ejecutar los siguientes comandos

```shell
https://blackarch.org/faq.html
chmod +x strap.sh
sudo ./strap.sh  # Importante ejecutarlo como sudo


# Luego estos pasos puede que algunos de fallos o necesiten de reinicar antes la pc

reboot                           # No he comprobado que sea necesario pero me ha ayudado
rm -rf /etc/pacman.d/gnupg       # Este comando si da algun fallo de "recurso ocupado" lo intentamos varias veces, si no lo saltamos
sudo pacman-key --init
sudo pacman-key --populate archlinux blackarch
sudo pacman-key --update --keyserver keyserver.ubuntu.com
```

3. Desinstalando paquetes extras

Si al momento de ejecutar la actualizacion final que compruebe que todo esta bien: `sudo pacman -Syu --noconfirm` nos da algun fallo como este:

```shell
 no se pudo descargar wireshark-qt-4.2.0-2-x86_64.pkg.tar.zst
```

Lo que debemos hacer sera desinstalarlos manualmente, aqui algunos paquetes que me han dado conflictos pero puede que sean mas:


```shell
sudo su                         # Volvernos sudo para este proceso tedioso

pacman -Rsc kconfig
pacman -Rsc jre-openjdk
pacman -Rsc wireshark-qt wireshark-cli
```

Si nos da un fallo de firmas aun, ejecutamos lo siguiente:

```shell
sudo pacman -Syy --noconfirm && sudo pacman-key --refresh-keys && sudo pacman-key --populate archlinux && sudo pacman-key --populate blackarch
```


Por ultimo si queremos que todo se instale mas rapido podemos usar: 

```shell
sudo vim  /etc/pacman.conf

# Agregamos esta linea en cualquier lugar del archivo:

ParallelDownloads = 20           # Esto sirve para descargas paralelas y por ende mas rapidas (depende de la velocidad del internet)
```


Podremos ejecutar: `sudo pacman -Syu --noconfirm` para comprobar que todo este bien, asi podremos tener BlackArch linux instalado!
