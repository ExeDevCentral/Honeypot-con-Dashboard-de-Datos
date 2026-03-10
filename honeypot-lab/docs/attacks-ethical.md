# Simulación de ataques (ético y local)

## Reglas del lab (no negociables)

- Solo atacás **tu propio entorno** (tu PC, tu VM, tu red de laboratorio).
- Usá **rates bajos** (no satures tu máquina ni tu red).
- Documentá fecha/hora, IP origen, herramienta usada y objetivo.

## Objetivo técnico

Generar eventos reales para que Cowrie capture:

- **Intentos de login** (usuarios/contraseñas comunes)
- **Comandos post-login** (enumeración, descarga, persistencia “falsa”)
- **Fingerprinting** (escaneo de puertos/servicios)

## Opción A: Manual (lo más didáctico)

```powershell
ssh -p 2222 root@localhost
```

Luego probá credenciales simples y tirá comandos típicos: `uname -a`, `id`, `cat /etc/passwd`, etc.

## Opción B: Escaneo básico

Desde una VM (Kali/Ubuntu) en la misma red:

- Escaneá **solo** el puerto `2222` para ver el servicio/banner.

## Opción C: Fuerza bruta controlada

Recomendación para estudiar: **pocos intentos**, listas chicas, y pausas.

Qué mirar en Kibana:

- `src_ip`, `username`, `password`, `event_id`

