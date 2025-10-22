# Maquina Trust

Dificultad : Muy Facil

## Fase de Reconocimiento
Vamos realizar el escaneo como la primera etapa, con un comando general con nmap sobre la ip de la victima para identificar los puertos que estan abiertos.
```
nmap -p- --open -sT --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
```

```
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-21 23:08 EDT
Initiating Connect Scan at 23:08
Scanning 172.17.0.2 [65535 ports]
Discovered open port 22/tcp on 172.17.0.2
Discovered open port 80/tcp on 172.17.0.2
Completed Connect Scan at 23:08, 1.65s elapsed (65535 total ports)
Nmap scan report for 172.17.0.2
Host is up, received user-set (0.00010s latency).
Scanned at 2025-10-21 23:08:23 EDT for 1s
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack

Read data files from: /usr/share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.69 seconds
```

### Parámetros Desglosados:

`-p-`
- Escanea **todos los puertos** (1-65535)

`--open`
- Muestra **solo puertos abiertos**, omite los filtrados/cerrados

`-sT`
- **TCP Connect Scan** - Establece conexión TCP completa
- No requiere privilegios root, pero es más lento y detectable

`--min-rate 5000`
- **Velocidad mínima** de 5000 paquetes por segundo
- Escaneo agresivo y rápido

`-vvv`
- **Triple verbose** - Máximo nivel de detalle en output
- Muestra información extensa del progreso

`-n`
- **Sin resolución DNS** - Más rápido, no hace lookups DNS

`-Pn`
- **No host discovery** - Trata el host como activo sin hacer ping

`-oG allPorts`
- **Output Grepable** - Guarda resultados en formato grepable en archivo "allPorts"

Ahora vamos a realizar un escaneo mas profundo con el siguiente comando.

```
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-21 23:12 EDT
Nmap scan report for 172.17.0.2
Host is up (0.000036s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 19:a1:1a:42:fa:3a:9d:9a:0f:ea:91:7f:7e:db:a3:c7 (ECDSA)
|_  256 a6:fd:cf:45:a6:95:05:2c:58:10:73:8d:39:57:2b:ff (ED25519)
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.57 (Debian)
MAC Address: 02:42:AC:11:00:02 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.01 seconds
```


### Parámetros Desglosados:

`-sC`
- Ejecuta **scripts por defecto** de NSE (Nmap Scripting Engine)
- Realiza pruebas de seguridad y recolección de información automática

`-sV` 
- **Detección de versión** de servicios
- Identifica qué software y versión corre en los puertos

`-p22,80`
- Escanea **puertos específicos**: 22 (SSH) y 80 (HTTP)
- Solo examina estos servicios, no todos los puertos

`-oN targeted`
- **Output Normal** - Guarda resultados en formato normal en archivo "targeted"


