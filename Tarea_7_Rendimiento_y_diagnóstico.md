# Tarea 7. Rendimiento y diagnóstico

## 2. Herramientas utilizadas
| Sistema | Herramientas |
|---|---|
| Windows anfitrión | Administrador de tareas, Monitor de recursos, Monitor de rendimiento |
| Windows Server / Windows 11 | Administrador de tareas, Visor de eventos, `Get-Process` |
| Linux | `top`, `htop`, `free -h`, `vmstat`, `iostat`, `sar`, `journalctl`, `df -h`, `ip -s link` |
| Proxmox | Panel web y estadísticas del nodo |
| TrueNAS | Panel de control y estadísticas |

## 3. Línea base en reposo
| Fecha/hora | CPU (%) | RAM usada/disponible | Disco (actividad/espacio) | Red (tráfico) | Temperatura si disponible |
|---|---:|---|---|---|---|
| 22/11/2026 23:10:49 | 4,9% | 7,4Gi / 23Gi | Usado: 29G/121G (26%) / Libre: 86G | wlp3s0 (RX: 2553,68 MB / TX: 75,34 MB) | 40,0 °C |

## 4. Mediciones por escenarios
| Escenario | Máquinas encendidas | CPU (%) | RAM (%) | Disco | Red | ¿El equipo responde correctamente? |
|---|---|---:|---:|---|---|---|
| 0 | Reposo | [ ] | [ ] | [ ] | [ ] | [ ] |
| 1 | OPNsense | [ ] | [ ] | [ ] | [ ] | [ ] |
| 2 | OPNsense + DebianInd | [ ] | [ ] | [ ] | [ ] | [ ] |
| 3 | OPNsense + Debian + Ubuntu_Server + Windows_Server + TrueNAS | [ ] | [ ] | [ ] | [ ] | [ ] |
| 4 | Anterior + Proxmox_1 + Windows_11 + Linux_Mint | [ ] | [ ] | [ ] | [ ] | [ ] |
| 5 | Máximo entorno estable probado | [ ] | [ ] | [ ] | [ ] | [ ] |

Indicar duración de cada medición, aplicaciones o servicios activos y condiciones de la prueba: [COMPLETAR]

## 5. Consumo por máquina virtual
| Máquina | CPU observada | RAM observada | Disco / red | Impacto en el anfitrión |
|---|---|---|---|---|
| OPNsense / MikroTik | [ ] | [ ] | [ ] | [ ] |
| Windows Server | [ ] | [ ] | [ ] | [ ] |
| Proxmox 1 / 2 / 3 | [ ] | [ ] | [ ] | [ ] |
| TrueNAS | [ ] | [ ] | [ ] | [ ] |
| Debian | [ ] | [ ] | [ ] | [ ] |
| Ubuntu Server | [ ] | [ ] | [ ] | [ ] |
| Windows 11 | [ ] | [ ] | [ ] | [ ] |
| Linux Mint | [ ] | [ ] | [ ] | [ ] |
