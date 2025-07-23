---
layout: single
title: "Laboratorio: Skill Check Recon & Enumeration"
date: 2025-07-23 20:20:00 +0000
author: Kazdov
excerpt: "Resolucion de laboratorio que requiere saber bien Recon & Enumeration"
categories:
  - ciberseguridad
tags:
  - pasivo
  - activo
  - enum
  - recon
---

---
## Aclaracion

Disculpen la demora para publicar un posteo, estuve muy ocupado estudiando mas a fondo el certificado y enfocandome en ello. Este blog es simplemente un sub producto de mis anotaciones y a modo de bitacora personal. Habiendo dicho eso, procedo a contarles como fue el laboratorio.

## 🎯 Objetivo

Realizar reconocimiento activo y pasivo sobre el host, identificar servicios abiertos, explorar posibles rutas expuestas, acceder a servidores vulnerables y capturar 4 flags.
Cada flag tiene formato: `FLAGX_md5hash`.

---

## 🛠️ Herramientas usadas

- `nmap`
- `ftp`
- `curl`
- `mysql` (CLI)
- Explorador web
- Sentido común y un poco de darle vuelta a las cosas.

---

## 🏁 Flag 1 – HTTP Header Disclosure

**🧩 Pista:**
*"El servidor anuncia su identidad en cada respuesta. Fijate bien, hay algo raro."*

**📌 Solución:**

```bash
curl -I http://target.ine.local
```

📥 El header `Server:` trae directamente el flag.
Literalmente estaba en la frente del server.

**✅ Lección:**
Los headers HTTP pueden filtrar info sensible, como versiones de software o incluso el flag en mi caso.

---

## 🏁 Flag 2 – robots.txt y el archivo escondido

**🧩 Pista:**
*"Las instrucciones del portero revelan lo que debería estar oculto."*

**📌 Solución:**

```bash
curl http://target.ine.local/robots.txt
```

Vimos `/secret-info/` como ruta bloqueada. Fui ahí, y apareció un `flag.txt`.

```bash
curl http://target.ine.local/secret-info/flag.txt
```

**✅ Lección:**
`robots.txt` es una mina de oro para pentesters. No siempre es un candado, muchas veces es una guía de lo que NO quieren que veas. Si hay algo que no quieren que veas echale un ojo igual.

---

## 🏁 Flag 3 – FTP abierto con acceso anónimo

**🧩 Pista:**
*"El acceso anónimo a veces lleva a tesoros olvidados."*

**📌 Solución:**

```bash
nmap -Pn -p- -A target.ine.local
```

Vimos FTP en el puerto 21 con `Anonymous login allowed`.

```bash
ftp target.ine.local
# user: ftp
# password: (Enter vacío)
```

Luego:

```bash
ls
get flag.txt
cat flag.txt
```

**✅ Lección:**
Si hay FTP, hay que testear login anónimo.
Muchas veces los admins ni saben que está habilitado.
Y a veces... te dejan **credenciales de base de datos** 👀

---

## 🏁 Flag 4 – MySQL exposed y mal configurado

**🧩 Pista:**
*"Una base bien nombrada puede revelar secretos."*

**📌 Usamos las creds del FTP:**

```text
db_admin:password@123
```

Y conectamos:

```bash
mysql -h target.ine.local -u db_admin -p
# password: password@123
```

Después:

```sql
SHOW DATABASES;
```

**✅ Lección:**
Si encontrás credenciales tiradas por ahí, probalas.
FTP y bases de datos mal configuradas van de la mano más seguido de lo que parece.

---

## 📚 Reflexiones

No todo está en el contenido del curso.
Este lab te pide perseverancia, prueba y error, lógica.
Esto se resuelve **jugando**, **deduciendo**, y **tocando todo** lo que se pueda.

Aprendí que “reconocimiento” es más que tirar `nmap` por todos lados, sino mas bien explorar pacientemente.
Es pensar cómo se construyen las cosas del lado del sysadmin, y cómo puede fallar.

Muchas de las cosas que usé (como `ftp` o `robots.txt`) no son técnicas avanzadas, pero sí **muy efectivas**.

---

## ✅ Cosas que puedo reforzar

- Saber mejor **qué mirar en headers HTTP**
- Practicar más con `ftp`, `nmap` y `mysql CLI`
- Familiarizarme con herramientas de enum como `gobuster`, `dirb`, `ffuf`, `whatweb`
- Hacer más labs **sin pedir ayuda al principio**. Darme más tiempo para pensar.

---

*Posteado por kazdov 🧠💥*
