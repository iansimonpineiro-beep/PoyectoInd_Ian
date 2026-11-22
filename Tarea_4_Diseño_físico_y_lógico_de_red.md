# Tarea 4. Diseño físico y lógico de red

## 2. Topología y direccionamiento
En clase se usa una red WAN compartida `192.168.40.0/24` con la IP que da el profesor. Mi red individual es `192.168.46.0/24`.

Mi posición / XX: 46  
Red LAN individual: 192.168.46.0/24 (máscara 255.255.255.0)  
IP WAN DHCP o profesor
Puerta de enlace: 192.168.46.1 (OPNsense). MikroTik, en 192.168.46.2, hace de puerta alternativa cuando lo pruebo.  
DNS: 192.168.46.20 (Windows Server) con reenvío al DNS del aula.

## 4. Diagrama físico o virtual de VirtualBox
`![Red VirtualBox](../diagramas/red-virtualbox.png)`

Tengo 11 máquinas creadas, todas apagadas: OPNsense, Windows_11, TrueNAS, Windows_Server, Linux_Mint, Proxmox_1, Proxmox_2, Proxmox_3, DebianInd, Ubuntu_Server y `vm` (tipo Linux 2.6, que creo que es el MikroTik CHR; lo confirmo).

Mi plan es poner solo OPNsense en adaptador puente hacia la red del aula (WAN) y el resto en una red interna `lan-46`. Así lo que haga en mi red, como el DHCP, no sale a la red compartida.

| Máquina | Adaptador 1 | Adaptador 2 | Segmento | Observaciones |
|---|---|---|---|---|
| OPNsense | WAN: adaptador puente | LAN: red interna `lan-46` | Puente + interna |  |
| MikroTik (`vm`) | WAN: puente (solo en pruebas) | LAN: red interna `lan-46` | Puente + interna |  |
| DebianInd | LAN: red interna `lan-46` | No se usa | Interna | Puerta de enlace 192.168.46.1 |
| Ubuntu_Server | LAN: red interna `lan-46` | No se usa | Interna | |
| Windows_Server | LAN: red interna `lan-46` | No se usa | Interna | DNS y dominio |
| Proxmox_1, 2 y 3 | LAN: red interna `lan-46` | No se usa | Interna | Hay que activar la virtualización anidada y el modo promiscuo "Permitir todo" |
| TrueNAS | LAN: red interna `lan-46` | No se usa | Interna | Discos extra para los RAID de la Tarea 6 |
| Windows_11 | LAN: red interna `lan-46` | No se usa | Interna | DHCP |
| Linux_Mint | LAN: red interna `lan-46` | No se usa | Interna | DHCP |

## 5. Tabla completa de interfaces
| Equipo | Interfaz | IP | Prefijo | Puerta de enlace | DNS | DHCP/estática | Segmento |
|---|---|---|---|---|---|---|---|
| OPNsense | WAN | 192.168.46.1 | /24 | — | 192.168.46.20 | Estática | lan-46 |
| OPNsense | LAN | — | /24 | — | Manual | DHCP |  |
| MikroTik | LAN | 192.168.46.2 | /24 | 192.168.46.1 | 192.168.46.20 | Estática | lan-46 |
| Debian | enp | 192.168.46.10 | /24 | 192.168.46.1 | 192.168.46.20 | Estática | lan-46 |
| Ubuntu Server | enp | 192.168.46.11 | /24 | 192.168.46.1 | 192.168.46.20 | Estática | lan-46 |
| Windows Server | Ethernet | 192.168.46.20 | /24 | 192.168.46.1 | 127.0.0.1 | Estática | lan-46 |
| Proxmox 1 | vmbr0 | 192.168.46.31 | /24 | 192.168.46.1 | 192.168.46.20 | Estática | lan-46 |
| Proxmox 2 | vmbr0 | 192.168.46.32 | /24 | 192.168.46.1 | 192.168.46.20 | Estática | lan-46 |
| Proxmox 3 | vmbr0 | 192.168.46.33 | /24 | 192.168.46.1 | 192.168.46.20 | Estática | lan-46 |
| TrueNAS | gestión | 192.168.46.40 | /24 | 192.168.46.1 | 192.168.46.20 | Estática | lan-46 |
| Windows 11 | Ethernet | 192.168.46.100-199 (rango propuesto, confirmar) | /24 | 192.168.46.1 | 192.168.46.20 | DHCP | lan-46 |
| Linux Mint | Ethernet | 192.168.46.100-199 (rango propuesto, confirmar) | /24 | 192.168.46.1 | 192.168.46.20 | DHCP | lan-46 |

## 6. Identificación de equipos y servicios
| Equipo | Función | SO | IP de gestión | Red | Virtual/físico |
|---|---|---|---|---|---|
| OPNsense | Cortafuegos, NAT y DHCP | OPNsense | 192.168.46.1 | WAN + LAN | Virtual |
| MikroTik | Router alternativo | RouterOS (CHR) | 192.168.46.2 | LAN | Virtual |
| Debian | Infraestructura / alternativa de comunicaciones | Debian | 192.168.46.10 | LAN | Virtual |
| Ubuntu Server | Infraestructura (p. ej. monitorización) | Ubuntu Server | 192.168.46.11 | LAN | Virtual |
| Windows Server | Dominio y DNS | Windows Server 2025 | 192.168.46.20 | LAN | Virtual |
| Proxmox 1-3 | Nodos de virtualización | Proxmox VE | 192.168.46.31-33 | LAN | Virtual (anidada) |
| TrueNAS | Almacenamiento | TrueNAS | 192.168.46.40 | LAN | Virtual |
| Windows 11 | Cliente | Windows 11 Pro | DHCP | LAN | Virtual |
| Linux Mint | Cliente | Linux Mint | DHCP | LAN | Virtual |

## 8. Verificación de conectividad (ampliación)
| Origen | Destino | Prueba | Resultado real | Observaciones |
|---|---|---|---|---|
| Windows 11 | 192.168.46.1 | `ping` | [Pendiente] | |
| Windows 11 | Internet autorizado | `tracert` | [Pendiente] | Tiene que pasar por 192.168.46.1 |
| Debian | 192.168.46.1 | `ip route` | [Pendiente] | Ruta por defecto vía .1 |
| Windows 11 | un nombre de dominio | `nslookup` | [Pendiente] | DNS 192.168.46.20 |
| Servidores | entre sí | `ping` | [Pendiente] | |
