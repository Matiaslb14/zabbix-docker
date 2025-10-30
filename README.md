# 🛰️ Zabbix 7 + Docker + GNS3 (SNMP)  
### (Laboratorio de Monitoreo con Zabbix 7, Docker y GNS3)

---

## 🧩 Overview / Descripción  

**EN:**  
A small lab that monitors Cisco devices (IOSv / IOSvL2 in GNS3) using **Zabbix 7 LTS on Docker**.  
It includes server, web UI, PostgreSQL and Agent2. SNMP hosts (R1 and SW-L3) are added, along with a dashboard showing **CPU / Memory / Traffic**.

**ES:**  
Un laboratorio pequeño que monitorea dispositivos Cisco (IOSv / IOSvL2 en GNS3) usando **Zabbix 7 LTS en Docker**.  
Incluye el servidor, interfaz web, PostgreSQL y Agent2. Se agregan hosts SNMP (R1 y SW-L3) y un panel que muestra **CPU / Memoria / Tráfico**.

---

## 📸 Screenshots / Capturas  

### 🌐 Network Topology / Topología de Red  
<p align="center">
  <img src="images/topologia.png" alt="Topology" width="800">
</p>

---

### 🛰️ SNMP Agents up / Agentes SNMP activos  
<p align="center">
  <img src="images/agentes_snmp_activos.png" alt="SNMP Agents Active" width="800">
</p>

---

### 📊 CPU & RAM Dashboard (R1) / Panel CPU y RAM (R1)  
<p align="center">
  <img src="images/cpu_ram_r1.png" alt="CPU and RAM R1 Dashboard" width="800">
</p>

---

## 📌 What you get / Qué obtienes  

| Feature (EN) | Descripción (ES) |
| ------------- | ---------------- |
| R1 and SW-L3 monitored over **SNMP** | R1 y SW-L3 monitoreados mediante **SNMP** |
| Dashboard with **CPU and Memory** metrics | Panel con métricas de **CPU y Memoria** |
| Data collected via Zabbix Agent2 + SNMP traps | Datos recolectados mediante Zabbix Agent2 y traps SNMP |
| Full lab integration with **GNS3 + Docker** | Integración completa del laboratorio con **GNS3 + Docker** |

---

## ✅ Requirements / Requisitos  

- Debian 12 / Ubuntu 22.04 (o similar)  
- Docker + Docker Compose  
- GNS3 con IOSv / IOSvL2 (o cualquier equipo con soporte SNMP)  

---

## 🔧 Environment Variables / Variables de Entorno  

Create a **`.env`** file (start from this example):  

**EN:**  
env
# .env.example
TZ=America/Santiago
POSTGRES_USER=zabbix
POSTGRES_PASSWORD=zabbix
POSTGRES_DB=zabbix

**ES:**
Crea un archivo .env basado en este ejemplo.

🚀 Deploy / Despliegue

**EN:**

docker compose up -d

Web UI →
http://<your-server-ip>:8080 (use HTTP, not HTTPS)
Default login → Admin / zabbix

**ES:**

docker compose up -d

Interfaz Web →
http://<tu-ip-servidor>:8080 (usa HTTP, no HTTPS)
Credenciales por defecto → Admin / zabbix

🌐 Connectivity to GNS3 Lab / Conectividad con el Laboratorio GNS3

**EN:**
If your VM has two NICs (NAT + host-only), add a static route to reach R1:

sudo ip route add 10.10.10.0/30 via 192.168.10.1 dev <your_hostonly_iface>

**ES:**
Si tu máquina virtual tiene dos interfaces (NAT + host-only), agrega una ruta estática para llegar a R1:

sudo ip route add 10.10.10.0/30 via 192.168.10.1 dev <tu_iface_hostonly>

🛰️ SNMP Configuration / Configuración SNMP

Cisco Devices (R1 / SW-L3):

conf t
 snmp-server community ZBXRO RO
 snmp-server ifindex persist
 snmp-server contact lab
 snmp-server location gns3
end
wr

In Zabbix:

SNMP v2

Community → ZBXRO

➕ Add Hosts / Agregar Hosts

| Step (EN)                              | Paso (ES)                                |
| -------------------------------------- | ---------------------------------------- |
| Data collection → Hosts → Create host  | Colección de datos → Hosts → Crear host  |
| Host name: R1                          | Nombre del host: R1                      |
| Interfaces → SNMP: 10.10.10.1 port 161 | Interfaces → SNMP: 10.10.10.1 puerto 161 |
| Template: Cisco IOS by SNMP            | Plantilla: Cisco IOS by SNMP             |
| Enable                                 | Habilitar                                |

Repeat for SW-L3 using IP 192.168.10.1.
Metrics appear within 1–2 minutes.

📊 Dashboard Setup / Configuración del Panel

**EN:**
Create dashboard → “LAB – Overview” → Add widgets for CPU, Memory, and Interface traffic.

**ES:**
Crea un panel → “LAB – Overview” → Agrega widgets para CPU, Memoria y tráfico de interfaces.

Use refresh every 10–30 seconds for real-time visualization.

🧪 Useful Commands / Comandos Útiles

docker compose ps
docker logs -f zbx-server
docker logs -f zbx-web

🧯 Troubleshooting / Solución de Problemas

| Problem (EN)                                                       | Solución (ES)                                                         |
| ------------------------------------------------------------------ | --------------------------------------------------------------------- |
| AppArmor errors with Agent2                                        | Error de AppArmor con Agent2                                          |
| → Add: `security_opt: - apparmor:unconfined` in docker-compose.yml | → Agrega: `security_opt: - apparmor:unconfined` en docker-compose.yml |
| SSL error (8080)                                                   | Error SSL (8080)                                                      |
| → Use HTTP, not HTTPS                                              | → Usa HTTP, no HTTPS                                                  |
| Ping to R1 fails                                                   | No se puede hacer ping a R1                                           |
| → Add static route (see above)                                     | → Agrega ruta estática (ver arriba)                                   |

📁 Repository Layout / Estructura del Repositorio

.
├── docker-compose.yml
├── .env.example
├── images/
│   ├── Agentes SNMP activos.png
│   ├── CPU, RAM R1.png
│   └── Topologia.png
└── README.md

👨‍💻 Developed by / Desarrollado por Matías Lagos Barra — Cloud & DevSecOps Engineer
