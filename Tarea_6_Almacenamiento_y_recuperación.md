# Tarea 6. Almacenamiento y recuperación

## 1. Comparación de sistemas de archivos
| Sistema | Dónde se usa | Características | Ventajas | Limitaciones | Uso previsto |
|---|---|---|---|---|---|
| NTFS | Windows | Journaling, ACL, cifrado, compresión y cuotas | Permisos detallados y fiable | Otros sistemas lo soportan peor | Discos de Windows Server y Windows 11 |
| FAT32 | Medios extraíbles | Simple y muy compatible | Funciona en casi todo | Archivos de máximo 4 GB, sin permisos | USB pequeños y la partición EFI |
| exFAT | Medios extraíbles | Pensado para memorias flash | Sin el límite de 4 GB y compatible con varios sistemas | Sin permisos ni journaling | Pasar archivos grandes |
| ext4 | Linux | Journaling y permisos POSIX | Estable y rápido | No tiene sumas de comprobación ni instantáneas | Debian, Ubuntu y Mint |
| XFS | Linux / servidores | Journaling, bueno con ficheros grandes | Escala bien | No se puede reducir | Servidores con archivos grandes |
| ZFS | TrueNAS | Sistema de archivos y gestor de volúmenes a la vez; sumas de comprobación, instantáneas, compresión | Detecta datos corruptos y permite replicar | Gasta más RAM y es más complejo | Pools de TrueNAS |

## 2. Práctica individual: creación y verificación de tres RAID
| Prueba | RAID | Discos virtuales | Herramienta |
|---|---|---|---|---|---|
| 1 | RAID 0 | 2 de 2 GB (esperado ≈ 4 GB) | `mdadm --level=0` o pool *stripe* |
| 2 | RAID 1 | 2 de 2 GB (≈ 2 GB) | `mdadm --level=1` o pool *mirror* |
| 3 | RAID 5 | 3 de 2 GB (≈ 4 GB) | `mdadm --level=5` o RAIDZ1 |

## 6. Plan de contingencia
| Incidencia | Afectado | Copia necesaria | Procedimiento | Prueba funcional |
|---|---|---|---|---|---|
| Fallo de una VM | VM de Proxmox o VirtualBox | Copia `vzdump` o exportación OVA | Restaurar desde la copia | La aplicación responde |
| Se pierde la configuración de red | OPNsense / servidores | `config.xml` y ficheros de red | Importar la configuración | Ping y salida a Internet |
| Falla un disco de datos | Volumen RAID | Copia de los datos en otro sitio | Cambiar el disco, reconstruir y restaurar si hace falta | Archivos íntegros (checksum) |
| Falla TrueNAS | Almacenamiento | Configuración exportada y copia de datos | Reinstalar, importar configuración y pool | Acceso a las carpetas compartidas |
| Error de actualización | Cualquier servidor | Instantánea previa | Volver a la instantánea | El servicio funciona |
