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
| 0 | Reposo | 4,9% | 7,4Gi / 23Gi | Usado: 29G/121G (26%) / Libre: 86G | wlp3s0 (RX: 2553,68 MB / TX: 75,34 MB) | Si |
| 1 | OPNsense | 5,9% | 10Gi / 20Gi | Usado: 29G/121G (26%) / Libre: 86G | wlp3s0 (RX: 2759,47 MB / TX: 78,80 MB)| Si |
| 2 | OPNsense + DebianInd | 8,2% | 11Gi / 19Gi | Usado: 29G/121G (26%) / Libre: 86G | wlp3s0 (RX: 2773,45 MB / TX: 79,38 MB) | Si |
| 3 | OPNsense + Debian + Ubuntu_Server + Windows_Server + TrueNAS | 2,1% | 12Gi / 17Gi | Usado: 29G/121G (26%) / Libre: 86G | wlp3s0 (RX: 2801,44 MB / TX: 80,53 MB) | Si |
| 4 | Máximo entorno estable probado | 2,1% | 12Gi / 17Gi | Usado: 29G/121G (26%) / Libre: 86G | wlp3s0 (RX: 2801,44 MB / TX: 80,53 MB) | Si |
