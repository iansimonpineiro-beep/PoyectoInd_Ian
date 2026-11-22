# Tarea 2. Equipos y periféricos

## 1. Comparación de los dos equipos
| Recurso | Ordenador del aula | Ordenador personal |
|---|---|---|
| CPU | i5 Ultra | Rycen 7 |
| RAM | 32 GB | 64 GB |
| Disco y espacio libre | 1 Tera | 1 Tera |
| Red | intel arc | NVIDIA GeForce RTX 5070 |

## 2. Cuellos de botella
| Recurso | Solución o mejora propuesta |
|---|---|
| CPU | Reducir vCPU por VM o encender menos máquinas a la vez. |
| RAM | Ampliar RAM o reducir la memoria asignada a VM poco usadas. |
| Disco | Migrar los discos virtuales a un SSD. |
| Red | Usar red interna de VirtualBox para el tráfico entre VM. |
| Temperatura / alimentación | Mejorar ventilación; usar SAI. |
