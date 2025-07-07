---
layout: post
title: "Footprinting y escaneo activo con Nmap"
date: 2025-07-07 16:00:00 +0000
---

Hoy estuve probando comandos de Nmap para hacer footprinting y escaneo activo.

Primero usé:

```bash
nmap -sn -PS 80 target
Con esto buscaba ver si había un host activo usando el puerto 80. Después, cuando confirmé que había algo, escaneé puertos más interesantes como:

nmap -p 21,22,445,8080,3389 target
También encontré una máquina Windows, así que escaneé directamente usando:

nmap -Pn -p 80,135,139,445,3389,49154,593 target
Esto me mostró varios puertos abiertos que suelen aparecer en entornos Windows.

Finalmente, usé el flag -sV para ver qué servicio estaba corriendo y su versión:

nmap -sV -p 80 target

Observaciones
A veces no me mostraba nada usando -sn, así que pasé directamente a -Pn.

En un caso asumí que había un firewall o algo que filtraba los pings.

Escaneé puertos conocidos de Windows y varios estaban abiertos, como 135, 139, 445, 3389 y 49154.

Con -sV me mostró qué servicio se estaba ejecutando en el puerto 80.

Notas
Me gustaría probar más sobre detección de sistema operativo más adelante (-O) y practicar con otras técnicas como banner grabbing. Por ahora, esto fue una buena práctica básica para entender mejor cómo identificar hosts y servicios en una red.
