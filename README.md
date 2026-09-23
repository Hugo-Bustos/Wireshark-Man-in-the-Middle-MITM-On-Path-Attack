# Análisis de Red y Detección de Ataques MITM (Man-in-the-Middle)

![Badge Ciberseguridad](https://img.shields.io/badge/Ciberseguridad-Blue?style=for-the-badge)
![Badge Wireshark](https://img.shields.io/badge/Wireshark-Blue?style=for-the-badge)
![Badge TryHackMe](https://img.shields.io/badge/TryHackMe-Lab-Red?style=for-the-badge)

*Nota: Este análisis fue realizado en un entorno de laboratorio controlado. Las técnicas documentadas tienen fines estrictamente educativos y de investigación defensiva.*

## Resumen del Escenario
Como parte de la investigación de una alerta de monitoreo de red en "Acme Corp", asumí el rol de Analista SOC para investigar un posible ataque Man-in-the-Middle (MITM) dentro de la LAN corporativa. 

A través del análisis de capturas de paquetes (`.pcap`) utilizando **Wireshark**, logré identificar y documentar una cadena de ataque completa que involucró tres fases:
1. **ARP Spoofing:** Intercepción del tráfico de la red.
2. **DNS Spoofing:** Redirección de resoluciones de dominio.
3. **SSL Stripping:** Degradación de cifrado para el robo de credenciales.

---

## Herramientas y Habilidades Demostradas
* **Análisis de Tráfico de Red:** Inspección profunda de paquetes (DPI) y lectura de archivos `.pcap`.
* **Manejo de Wireshark:** Creación de filtros avanzados para aislar tráfico malicioso entre el ruido de la red.
* **Troubleshooting de Redes:** Comprensión práctica de los protocolos ARP, DNS, HTTP y TLS/SSL.
* **Respuesta a Incidentes:** Trazabilidad de un ataque basándose en la *Cyber Kill Chain*.

---

## Resolución Paso a Paso

### Fase 1: Detectando el Envenenamiento ARP (Intercepción)
El atacante aprovecha la falta de autenticación del protocolo ARP para suplantar a la puerta de enlace predeterminada (`192.168.10.1`). Para aislar al atacante y excluir la dirección MAC legítima del router (`02:aa:bb:cc:00:01`), utilicé el siguiente filtro:

```text
arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1 && eth.src != 02:aa:bb:cc:00:01
