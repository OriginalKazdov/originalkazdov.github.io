---
layout: single
title: "Footprinting y reconocimiento en ciberseguridad (pasivo)"
date: 2025-07-08 20:20:00 +0000
author: Kazdov
excerpt: "Resumen y apuntes sobre las técnicas de recopilación pasiva de información en ciberseguridad."
categories:
  - ciberseguridad
  - info-gathering
tags:
  - footprinting
  - reconocimiento
  - pasivo
  - pentesting
---

El reconocimiento pasivo es una de las etapas más importantes dentro de un proceso de pentesting. Es la primera fase en la que comenzamos a recolectar información del objetivo **sin interactuar directamente con él**.

---

## ¿Qué significa que sea pasivo?

Quiere decir que en ningún momento enviamos tráfico al objetivo. No escaneamos sus servidores, ni interactuamos con sus sistemas. Solo **buscamos y analizamos datos públicos disponibles en Internet**.

> Si en algún punto hacemos contacto con el target (por ejemplo, con un escaneo Nmap o un curl a su web), ya estaríamos hablando de técnicas activas. Eso lo vemos en otro posteo.

---

## ¿Qué información buscamos?

El objetivo del footprinting es reunir **la mayor cantidad de información posible** para usar en las fases siguientes del pentest: escaneo, explotación y post-explotación.

Entre más información recolectemos ahora, **menos sorpresas** vamos a tener después. Por eso es importante ser minucioso cuando recolectamos información.

Podemos buscar:

- Direcciones IP públicas
- Registros DNS y subdominios
- Nombres de dominio y fechas de expiración
- Correos electrónicos relacionados al dominio
- Redes sociales
- Tecnología que usa la web (CMS, librerías JS)
- Archivos de configuración públicos (como `robots.txt`, `sitemap.xml`)

---

## Herramientas útiles para reconocimiento pasivo

| Herramienta       | Función                            | Tipo   |
|-------------------|------------------------------------|--------|
| `whois`           | Información del dominio y dueño    | Pasiva |
| `host`            | DNS lookups                        | Pasiva |
| `dig`             | Consultas DNS avanzadas            | Pasiva |
| `theHarvester`    | Emails, hosts y nombres            | Pasiva |
| `wafw00f`         | Detectar si hay un WAF (activo)    | Activa |
| `curl robots.txt` | Acceso al archivo de exclusiones   | Pasiva |

---

## Ejemplo: usando `whois` y `host`

```bash
whois example.com
```
```bash
host example.com
```

Salida parcial:

```
example.com has address 93.184.216.34
example.com mail is handled by 0 .
```

---

## Explorando robots.txt

Este archivo está en la mayoría de las páginas web y le dice a los bots qué partes de la página no tienen que indexar.

```bash
curl -s https://example.com/robots.txt
```

Muchas paginas esconden rutas sensibles aca. Si ves rutas como `/admin`, `/secret`, o `/backup/`, tomá nota. No deberías accederlas sin autorización, pero saber que existen te da información valiosa.

---

## Conclusión

Esta fase es silenciosa pero poderosa. Es la base para todo el proceso de pentesting. No se subestima. ¡El footprinting bien hecho puede ahorrarte horas de trabajo posterior!
