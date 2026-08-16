🛡️ Live Incident Response — 4geeks-server

Proyecto final de respuesta a incidentes en tiempo real (Live Incident Response) sobre un servidor Ubuntu 20.04 LTS comprometido, realizado como Analista de Ciberseguridad para 4Geeks Academy.

📋 Descripción del incidente

El 23 de junio de 2025, el servidor 4geeks-server fue comprometido mediante una cadena de ataque que combinó fuerza bruta SSH, creación de cuentas backdoor, un script dropper y un mecanismo de persistencia vía cron que exfiltraba datos del sistema hacia un host externo cada 15 minutos.

El análisis se realizó sin apagar el servidor ni interrumpir los servicios en producción (Apache, SSH, FTP), dado que se trataba de un sistema crítico, siguiendo un enfoque de inspección directa sobre sistema activo.

🎯 Resumen de la cadena de ataque
Elemento	Detalle
Vector de entrada	Fuerza bruta SSH desde 192.168.1.103
Cuentas backdoor	hacker (UID 1002), reports (UID 1001) — sin privilegios sudo
Dropper	install.sh — descarga y ejecuta payload.bin desde 192.168.1.100
Persistencia	Cronjob root cada 15 min ejecutando /usr/local/bin/backup2.sh
Impacto	Exfiltración automatizada de /etc/passwd hacia 192.168.1.100:8080/upload
Anti-forense	Manipulación del bash_history para simular actividad legítima
📁 Contenido del repositorio
├── Informe_Respuesta_Incidentes_4geeks-server.docx   # Informe técnico final (3 fases)
├── evidencias/                                        # Capturas de pantalla (EVD-01 a EVD-40)
└── README.md
🔬 Metodología — 3 Fases
Fase 1: Reconocimiento y recolección de evidencias

Inspección en vivo del sistema activo: árbol de procesos, cuentas de usuario, logs de autenticación, reglas de firewall, tareas cron, servicios activos, y contenido íntegro de los artefactos maliciosos recuperados (backup2.sh, install.sh).

Fase 2: Remediación y restauración
Contención: bloqueo de comunicación con los hosts atacantes vía ufw.
Erradicación: eliminación de cuentas backdoor, credenciales expuestas, artefactos del atacante y el mecanismo de persistencia (cronjob + script).
Recuperación: verificación de que los servicios legítimos y procesos del sistema continúan operando sin anomalías.
Hardening aplicado: instalación de fail2ban, reglas de firewall específicas, endurecimiento de configuración SSH.
Fase 3: Recomendaciones a futuro

Acciones de prevención y hardening a mediano/largo plazo que exceden el alcance de la contención inmediata (autenticación por llave SSH, segmentación de accesos, auditoría de integridad, centralización de logs, etc.)

🗂️ Evidencia

Todas las capturas están identificadas con el código EVD-NN y referenciadas en el Anexo del informe, indicando el comando ejecutado y la sección del documento a la que corresponden (Fase 1 o Fase 2). Deben conservarse íntegras como parte de la cadena de custodia.

✅ Estado final

El incidente se considera contenido y remediado: se eliminó por completo el mecanismo de persistencia, se dieron de baja las cuentas no autorizadas, y se verificó la integridad y disponibilidad del sistema sin interrupción del servicio.

🧰 Herramientas y comandos clave utilizados

ps, ss, ip a, iptables / ufw, crontab, /etc/cron.d/, /var/log/auth.log, last, bash_history, /etc/passwd, /etc/sudoers, systemctl, dpkg, grep, find, userdel, fail2ban.

📚 Referencia

Guía de Investigación de Incidentes para Analistas Blue Team — 4Geeks

