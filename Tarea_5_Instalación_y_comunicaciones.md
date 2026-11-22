# Tarea 5. Instalación y comunicaciones

## 1. Inventario de sistemas instalados
| Sistema / máquina | Función | Arquitectura | RAM | Disco | Estado |
|---|---|---|---:|---|---|
| OPNsense | Comunicaciones | 64 bits | 4048 MB | 20 GB (IDE) | Apagada |
| MikroTik (`vm`) | Router alternativo | 64 bits | | | Apagada |
| TrueNAS | NAS/SAN | 64 bits | 8192 MB | 2 discos de 20 GB (SATA) | Apagada |
| Proxmox_1 | Virtualización | 64 bits | 2048 MB | 20 GB (SATA) | Apagada |
| Proxmox_2 y Proxmox_3 | Virtualización | 64 bits |  |  | Apagadas  |
| Windows_Server | Dominio / servicios | 64 bits | 4096 MB | 80 GB (SATA), EFI y TPM 2.0 | Apagada |
| DebianInd | Infraestructura | 64 bits | 1024 MB | 3 discos: 20 GB, 20,58 GB y 20 GB | Apagada |
| Ubuntu_Server | Infraestructura | 64 bits | 2048 MB | 25 GB (SATA) | Apagada |
| Windows_11 | Cliente | 64 bits | 4096 MB | 80 GB (SATA), EFI y TPM 2.0 | Apagada |
| Linux_Mint | Cliente | 64 bits | 2048 MB | 25 GB (SATA) | Apagada |



## 3. Registro de instalación por sistema

### OPNsense
- Función: cortafuegos/router, NAT y DHCP de la LAN
- Origen oficial de la imagen: https://opnsense.org/download/
- Integridad / checksum comprobado: Sí
- Recursos asignados a la VM: 2 vCPU, 4048 MB de RAM, disco `OPNsense.vdi` de 20 GB en controlador IDE
- Firmware y arranque: BIOS . Orden: disquete, óptica, disco duro.
- Aceleración: paginación anidada y paravirtualización KVM
- Usuario administrativo creado : root
- Configuración de red: adaptador 1 PCnet-FAST III en NAT y adaptador 2 PCnet-FAST III en red interna `intnet`. LAN prevista 192.168.46.1/24.

### MikroTik (VM `vm`)
- Función: router alternativo
- Origen oficial de la imagen: https://mikrotik.com/download (CHR)
- Usuario administrativo (no incluyo contraseña): admin
- Configuración de red: LAN prevista 192.168.46.2/24

### TrueNAS
- Función: almacenamiento (NAS/SAN)
- Origen oficial de la imagen: https://www.truenas.com/download/
- Recursos asignados: 2 vCPU, 8192 MB de RAM, `TrueNAS.vdi` y `TrueNAS_1.vdi` de 20 GB cada uno en SATA (puertos 0 y 1)
- Firmware y arranque: BIOS
- Red: solo se ve el adaptador 2, Intel PRO/1000 MT Desktop en red interna `intnet`. IP prevista 192.168.46.40/24.

### Proxmox (Proxmox_1, Proxmox_2 y Proxmox_3)
- Función: nodos de virtualización
- Origen oficial de la imagen: https://www.proxmox.com/downloads
- Recursos de Proxmox_1: 2 vCPU, 2048 MB de RAM, disco SATA de 20 GB. El fichero del disco se llama `Proxmox_2.vdi`.
- Red de Proxmox_1: adaptador 2 Intel PRO/1000 MT Desktop en red interna `intnet`. IP prevista 192.168.46.31/24.
- Aceleración: paginación anidada y paravirtualización KVM

### Windows Server
- Función: dominio y DNS
- Origen oficial de la imagen: Centro de evaluación de Microsoft
- Recursos asignados: 2 vCPU, 4096 MB de RAM, disco de 80 GB, TPM 2.0 y EFI habilitados, gráfica VBoxSVGA con 128 MB
- Imagen montada: una ISO llamada `w11.iso` (7,89 GB). Ver observación del apartado 7.
- Red: adaptador 1 Intel PRO/1000 MT Desktop en NAT. IP prevista 192.168.46.20/24 cuando pase a la red interna.

### Debian (DebianInd)
- Función: infraestructura / alternativa de comunicaciones
- Origen oficial de la imagen: https://www.debian.org/distrib/
- Recursos asignados: 1024 MB de RAM; vCPU no visible en la captura. Discos: `DebianInd.vdi` (20 GB)
- Red: adaptador 2 Intel PRO/1000 MT Desktop en red interna `intnet`. IP prevista 192.168.46.10/24.
- Los dos discos adicionales me sirven para las pruebas de RAID de la Tarea 6.

### Ubuntu Server
- Función: infraestructura
- Origen oficial de la imagen: https://ubuntu.com/download/server
- Recursos asignados: 2 vCPU, 2048 MB de RAM, disco `Ubuntu_Server.vdi` de 25 GB. La unidad óptica está vacía.
- Red: adaptador 1 Intel PRO/1000 MT Desktop en NAT. IP prevista 192.168.46.11/24 cuando pase a la red interna.

### Windows 11
- Función: cliente
- Imagen: `Win11_25H2_Spanish_x64.iso` (7,21 GB), descargada de https://www.microsoft.com/software-download/windows11
- Recursos asignados: 2 vCPU, 4096 MB de RAM, disco de 80 GB, TPM 2.0 y EFI habilitados
- Red: adaptador 1 Intel PRO/1000 MT Desktop en NAT. Pasará a la red interna con IP por DHCP.

### Linux Mint
- Función: cliente
- Imagen: `linuxmint-22.3-cinnamon-64bit.iso` (2,88 GB), de https://linuxmint.com/download.php
- Recursos asignados: 2 vCPU, 2048 MB de RAM, disco `Linux_Mint.vdi` de 25 GB
- Red: adaptador 1 Intel PRO/1000 MT Desktop en NAT. Pasará a la red interna con IP por DHCP.

## 4. Configuración por función

### 4.1 Servidor de comunicaciones
Sistema utilizado: OPNsense, MikroTik y Debian con nftables como alternativas  
Interfaces WAN y LAN: según las capturas, la WAN de OPNsense está en NAT de VirtualBox y la LAN en la red interna `intnet`  
IP LAN: 192.168.46.1/24  
DHCP (equipo e interfaz que lo proporciona): OPNsense, interfaz LAN, rango propuesto 192.168.46.100-199  

### 4.2 TrueNAS
Interfaz e IP de administración: 192.168.46.40/24  
Discos disponibles: 2 discos virtuales de 20 GB
Usuarios y permisos (sin contraseñas): admin
Copia de configuración: Sistema > Configuración general > Guardar configuración

### 4.3 Proxmox
IP de gestión y nombre del nodo: IPs 192.168.46.31, .32 y .33

### 4.4 Servidores GNU/Linux
Sistema y versión: Debian y Ubuntu Server
IP y nombre del equipo: Debian 192.168.46.10; Ubuntu Server 192.168.46.11  
Estado de servicios y registros: `systemctl status <servicio>` y `journalctl -u <servicio>`  
Archivos de configuración respaldados: `/etc/netplan` o `/etc/network/interfaces`, `/etc/ssh/sshd_config` y nftables

### 4.5 Clientes Windows 11 y Linux
Versiones: Windows 11 25H2 y Linux Mint 22.3
IP, máscara, gateway y DNS: por DHCP de OPNsense, /24, gateway 192.168.46.1, DNS 192.168.46.20


## 5. Copia y recuperación
- Elemento respaldado: `config.xml` de OPNsense y un VM de pruebas
- Tipo de copia: completa
- Destino: TrueNAS
- Procedimiento de recuperación: restaurar la configuración o la VM desde la copia y reiniciar

