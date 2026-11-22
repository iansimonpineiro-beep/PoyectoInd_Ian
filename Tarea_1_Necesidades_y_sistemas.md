# Tarea 1. Necesidades y sistemas

## 1. Objetivos
En esta tarea analizo qué necesidades presenta cada parte: comunicaciones, virtualización, gestión de usuarios, equipos cliente y almacenamiento. Para elegir me baso en requisitos, licencias, soporte.

| Necesidad | Qué necesita (dato o supuesto) | Cómo lo compruebo |
|---|---|---|
| Comunicaciones | Salida a Internet con NAT y la LAN `192.168.46.0/24`. Filtrado y DHCP en el mismo equipo. | Ping y traceroute desde un cliente; reglas de permitir y bloquear mirando el registro del cortafuegos. |
| Virtualización | Tres nodos Proxmox. Montarlos en clúster. | `pvecm status`, crear una VM y un contenedor y migrar una VM. |
| Gestión centralizada | Windows Server 2025 como dominio. Usuarios por departamentos. | Unir un cliente al dominio e iniciar sesión con un usuario del dominio. |
| Equipos cliente | 6 Windows 11 Pro y 2 Linux. | Que cada cliente resuelva nombres, navegue y acceda a las carpetas compartidas. |
| Datos y almacenamiento | TrueNAS. Una carpeta por departamento. | Montar una carpeta. |
| Copias y recuperación | Copias de configuraciones y de las VM importantes. | Restaurar una VM de prueba y comprobar que el servicio vuelve a funcionar. |
| Supervisión | Hay un servidor de monitorización. Vigilar CPU, RAM, disco y si el servicio está activo. | Apagar un servicio a propósito y ver si salta el aviso. |

### 2. Comunicaciones
| Criterio | MikroTik RouterOS/CHR | OPNsense | GNU/Linux con nftables |
|---|---|---|---|
| Qué es | Router y cortafuegos.| Cortafuegos con interfaz web. | Un Linux (Debian) configurado como router con nftables. |
| Requisitos mínimos oficiales | Muy bajos, unos 128 MB de RAM. | Unos 2 GB de RAM y 8-10 GB de disco. | Los de Debian sin escritorio, 512 MB-1 GB. |
| Recursos para la prueba | 1 vCPU, 256 MB, 1 GB de disco | 2 vCPU, 2 GB, 20 GB de disco | 1 vCPU, 1 GB, 10 GB de disco |
| Licencia y coste | CHR gratis pero limitado a 1 Mbit/s por interfaz. | Libre y gratuito. | Libre y gratuito. |
| Administración | Win, web y comandos. Tiene una sintaxis propia que cuesta al principio. | Web muy clara. | Solo comandos y ficheros. |
| Ventajas | Muy ligero y potente. | Fácil de usar y trae casi todo incluido. | Control total y sin coste. |
| Limitaciones | Límite de velocidad sin licencia. | Gasta más recursos. | Más trabajo a mano y más fácil equivocarse. |
| Compatibilidad con el laboratorio | Buena en VirtualBox. | Buena en VirtualBox. | Buena, ya tengo Debian montado. |
| Fuente | https://help.mikrotik.com | https://docs.opnsense.org | https://wiki.nftables.org |

### 3.1 Virtualización
Comparación Proxmox VE con VMware ESXi, Hyper-V y KVM con libvirt.

- **Proxmox VE:** gratuito, con interfaz web, máquinas virtuales y contenedores LXC, clúster y copias integradas. El soporte oficial es de pago. Para el quórum del clúster se recomiendan tres nodos.
- **VMware ESXi/vSphere:** muy maduro pero de pago.
- **Hyper-V:** va incluido con Windows Server y se integra bien con el dominio, pero necesita la licencia.
- **KVM con libvirt:** libre y flexible, pero no tiene una gestión unificada como Proxmox.

Fuente: https://pve.proxmox.com/pve-docs/

### 3.2 Gestión centralizada
Comparación Windows Server con Active Directory, Samba AD y FreeIPA.

Un dominio sirve para tener usuarios, grupos y equipos controlados desde un solo sitio: inicio de sesión único, permisos sobre carpetas, DNS y directivas de grupo. Lo que no hace es guardar los datos de los usuarios, hacer las copias de seguridad ni arreglar problemas de hardware o de red.

Samba AD es gratis pero tiene menos herramientas gráficas, y FreeIPA está pensado para entornos Linux. Como la mayoría de clientes son Windows 11, me quedo con Windows Server 2025. Requisitos: CPU 64 bits a 1,4 GHz, 2 GB de RAM y 32 GB de disco. En https://learn.microsoft.com/windows-server/

### 3.3 Clientes
Linux Mint.

| Criterio | Windows 11 | Linux Mint |
|---|---|---|
| Aplicaciones | Compatible con todo el software. | El software de Windows hay que sustituirlo. |
| Requisitos | 1 GHz y 2 núcleos, 4 GB de RAM, 64 GB de disco, UEFI y TPM 2.0. | Unos 2 GB de RAM y 20 GB de disco. |
| Licencia | Gratis. | Gratis. |
| Administración | Dominio y GPO nativos. | Se une al dominio con SSSD pero sin GPO. |

## 4. Decisiones justificadas
| Función | Necesidad | Opciones | Criterios y pruebas | Elegida | Inconvenientes que acepto |
|---|---|---|---|---|---|
| Comunicaciones | NAT, rutas, filtrado y DHCP | MikroTik, OPNsense, Debian con nftables | Facilidad, recursos y licencia; probar salida a Internet y reglas | OPNsense, y pruebo también MikroTik y Debian | Gasta más RAM que MikroTik |
| Virtualización | Alojar servicios con clúster | Proxmox | clúster; crear VM y migrar | Proxmox VE | Soporte oficial |
| Gestión centralizada | Usuarios y GPO | Windows Server | Compatibilidad con Windows 11; unir un equipo | Windows Server 2025 | Licencias |
| Clientes | Puestos de usuario | Windows 11, Linux Mint | Compatibilidad y requisitos | Windows 11 Pro, con Mint | Windows y más recursos |
| Datos | Carpetas | TrueNAS, OpenMediaVault, Windows | Integridad y protocolos | TrueNAS | Consume mucha RAM |
