<div align="center">

<img src="https://img.shields.io/badge/ZenFlow-v0.1.0--dev-2e7d32?style=for-the-badge&logo=leaf&logoColor=white" alt="version"/>
<img src="https://img.shields.io/badge/status-Phase_1-0288d1?style=for-the-badge" alt="status"/>
<img src="https://img.shields.io/badge/license-MIT-8bc34a?style=for-the-badge" alt="license"/>

<br/><br/>

```
███████╗███████╗███╗   ██╗███████╗██╗      ██████╗ ██╗    ██╗
╚══███╔╝██╔════╝████╗  ██║██╔════╝██║     ██╔═══██╗██║    ██║
  ███╔╝ █████╗  ██╔██╗ ██║█████╗  ██║     ██║   ██║██║ █╗ ██║
 ███╔╝  ██╔══╝  ██║╚██╗██║██╔══╝  ██║     ██║   ██║██║███╗██║
███████╗███████╗██║ ╚████║██║     ███████╗╚██████╔╝╚███╔███╔╝
╚══════╝╚══════╝╚═╝  ╚═══╝╚═╝     ╚══════╝ ╚═════╝  ╚══╝╚══╝
```

### 🌿 Sistema Acuapónico Automatizado para la Promoción del Autocuidado Mindful
### 🌿 Automated Aquaponics System for Mindfulness Support

<br/>

[![CI — Tests & Lint](https://img.shields.io/github/actions/workflow/status/your-org/zenflow-aquaponics/ci.yml?branch=develop&label=CI%20Tests&logo=github-actions&logoColor=white&style=flat-square)](https://github.com/your-org/zenflow-aquaponics/actions)
[![Firmware Build](https://img.shields.io/github/actions/workflow/status/your-org/zenflow-aquaponics/firmware.yml?label=Firmware%20Build&logo=arduino&logoColor=white&style=flat-square)](https://github.com/your-org/zenflow-aquaponics/actions)
[![codecov](https://img.shields.io/codecov/c/github/your-org/zenflow-aquaponics?style=flat-square&logo=codecov)](https://codecov.io/gh/your-org/zenflow-aquaponics)
[![Docker](https://img.shields.io/badge/Docker-ghcr.io-2496ed?style=flat-square&logo=docker&logoColor=white)](https://github.com/your-org/zenflow-aquaponics/pkgs/container/zenflow-aquaponics)

</div>

---

## ¿Qué es ZenFlow? / What is ZenFlow?

**ZenFlow** es un sistema acuapónico inteligente que combina **acuicultura, hidroponía y tecnología embebida** para crear un entorno multisensorial de apoyo al *Mindfulness* en oficinas y escuelas. Monitorea en tiempo real los parámetros del ecosistema, detecta el comportamiento de los peces con visión computacional y evalúa la salud de las plantas con análisis infrarrojo, todo unificado en un dashboard web local.

**ZenFlow** is a smart aquaponics system combining **aquaculture, hydroponics and embedded technology** to create a multisensory environment supporting *Mindfulness* in offices and schools. It monitors ecosystem parameters in real time, tracks fish behaviour with computer vision, and evaluates plant health with infrared analysis — all unified in a local web dashboard.

<br/>

## ✨ Características / Features

| | Feature | Detalle |
|---|---|---|
| 💧 | **15 variables sensadas** | pH, DO, T°, EC, NH₃, NO₂, NO₃, turbidez, nivel, flujo, humedad, CO₂, PAR, IR |
| 🐟 | **Tracking de peces con YOLO** | Detección + ByteTrack: posición, velocidad e Índice de Salud Acuícola (ISA) |
| 🌱 | **Salud de plantas (NDVI/IR)** | Cámara infrarroja → mapa térmico foliar + score NDVI en tiempo real |
| 📡 | **Mensajería eficiente** | RabbitMQ + Protocol Buffers entre ESP32, Raspberry Pi y servidor x86 |
| 📊 | **Dashboard web local** | React + FastAPI + PostgreSQL, accesible desde cualquier dispositivo en LAN |
| 🔄 | **CI/CD cross-platform** | GitHub Actions: tests en Windows, Ubuntu y macOS + firmware PlatformIO |

<br/>

## 🏗️ Arquitectura / Architecture

```
                         ┌─────────────────── x86 Server (LAN) ───────────────────┐
                         │                                                          │
  ┌─────────────┐        │  ┌──────────────┐    ┌──────────────┐    ┌───────────┐ │
  │   ESP32     │ MQTT/  │  │   RabbitMQ   │    │   FastAPI    │    │  React    │ │
  │  (Sensors)  │protobuf│  │   Broker     │───►│  + Alembic   │───►│ Dashboard │ │
  │  C++ / PIO  │───────►│  │              │    │  + PostgreSQL│    │    LAN    │ │
  └─────────────┘        │  └──────┬───────┘    └──────────────┘    └───────────┘ │
                         │         │                                               │
  ┌─────────────┐        │         │                                               │
  │ Raspberry Pi│ HTTP / │         │                                               │
  │  Webcam     │protobuf│         │                                               │
  │  IR Camera  │───────►│─────────┘                                               │
  │  Python     │        │                                                          │
  └─────────────┘        └──────────────────────────────────────────────────────────┘

  ESP32:  pH · DO · T° · EC · NH₃ · NO₂ · NO₃ · Turbidez · Nivel · Flujo
  RPi:    YOLOv8 + ByteTrack (fish) · IR/NDVI (plants) · DHT22 · BH1750 · CO₂
```

<br/>

## 🗂️ Estructura del proyecto / Project Structure

```
zenflow-aquaponics/
├── 📁 firmware/          # C++ — ESP32 con PlatformIO
├── 📁 rpi/               # Python — Raspberry Pi (visión + sensores)
├── 📁 protos/            # Protocol Buffers schemas (.proto)
├── 📁 backend/           # Python — FastAPI + PostgreSQL + consumidores RabbitMQ
├── 📁 dashboard/         # TypeScript — React + Vite
├── 📁 wiki/              # Documentación completa del proyecto
├── 📁 .github/workflows/ # CI/CD: ci.yml · cd.yml · firmware.yml · protobuf.yml
├── 🐳 docker-compose.yml
└── 📄 .env.example
```

<br/>

## 🚀 Inicio rápido / Quick Start

### Requisitos / Requirements

| Herramienta | Versión mínima |
|---|---|
| Docker + Docker Compose | v24+ |
| Python | 3.11+ |
| Node.js | 20 LTS |
| PlatformIO CLI | latest |
| Raspberry Pi OS | Bookworm (64-bit) |

### 1. Clonar el repositorio / Clone

```bash
git clone https://github.com/your-org/zenflow-aquaponics.git
cd zenflow-aquaponics
```

### 2. Configurar variables de entorno / Environment

```bash
cp .env.example .env
# Edita .env con tus parámetros de red, credenciales de DB y RabbitMQ
```

### 3. Levantar servicios / Start services

```bash
docker compose up -d
# FastAPI → http://localhost:8000
# Dashboard → http://localhost:5173
# RabbitMQ UI → http://localhost:15672
```

### 4. Flashear firmware ESP32

```bash
cd firmware
pio run --target upload
```

### 5. Iniciar visión en Raspberry Pi

```bash
cd rpi
pip install -r requirements.txt
python vision/fish_tracker.py --source /dev/video0
```

<br/>

## 📅 Fases del proyecto / Project Phases

```
2026  Apr ─────────── Jun          Jul──Aug        Sep ────────── Nov
      │                │           │    │           │               │
      ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓           ░░░░░░           ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓
      │  FASE 1         │           PAUSA            │  FASE 2      │
      │  Hardware &     │                            │  Software &  │
      │  Prototipado    │                            │  Integración │
      └─────────────────┘                            └──────────────┘
        13 abr–30 jun                                  30 ago–30 nov
```

| Fase | Periodo | Objetivo | Estado |
|---|---|---|---|
| **F1** | 13 abr – 30 jun | Hardware, firmware, sensores, visión básica | 🔵 En curso |
| **Pausa** | 1 jul – 29 ago | Sistema físico en operación pasiva | ⚪ Pendiente |
| **F2** | 30 ago – 30 nov | Backend, dashboard, CI/CD, Docker, v1.0 | ⚪ Pendiente |

> 📋 Ver cronograma completo con entregables cada 2 semanas → [`wiki/01-introduccion.md`](wiki/01-introduccion.md)

<br/>

## 🏁 Milestones

| # | Milestone | Fecha límite | Estado |
|---|---|---|---|
| M1 | Hardware Core & Sensor Firmware | 9 may 2026 | 🔵 Activo |
| M2 | Messaging Infrastructure (RabbitMQ + Protobuf) | 23 may 2026 | ⚪ Pendiente |
| M3 | Computer Vision — Fish Tracking | 30 jun 2026 | ⚪ Pendiente |
| M4 | Backend API & Database | 27 sep 2026 | ⚪ Pendiente |
| M5 | Dashboard Web & Plant Health Vision | 25 oct 2026 | ⚪ Pendiente |
| M6 | CI/CD, Docker & Release v1.0 | 30 nov 2026 | ⚪ Pendiente |

<br/>

## 🔄 CI/CD — GitHub Actions

| Workflow | Trigger | Plataformas |
|---|---|---|
| `ci.yml` | PR → `develop` / `main` | ✅ Windows · ✅ Ubuntu · ✅ macOS |
| `cd.yml` | Push tag `v*.*.*` | Ubuntu → GHCR |
| `firmware.yml` | Push a `firmware/**` | Ubuntu (PlatformIO) |
| `protobuf.yml` | Push a `protos/**` | Ubuntu → auto-commit bindings |

<br/>

## 🛠️ Stack tecnológico / Tech Stack

<div align="center">

![Python](https://img.shields.io/badge/Python-3.11-3776ab?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.111-009688?style=flat-square&logo=fastapi&logoColor=white)
![React](https://img.shields.io/badge/React-18-61dafb?style=flat-square&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178c6?style=flat-square&logo=typescript&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?style=flat-square&logo=postgresql&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-3.13-ff6600?style=flat-square&logo=rabbitmq&logoColor=white)
![Protobuf](https://img.shields.io/badge/Protocol_Buffers-v3-4caf50?style=flat-square&logo=google&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ed?style=flat-square&logo=docker&logoColor=white)
![ESP32](https://img.shields.io/badge/ESP32-PlatformIO-e7352c?style=flat-square&logo=espressif&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry_Pi-5-c51a4a?style=flat-square&logo=raspberry-pi&logoColor=white)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-7b68ee?style=flat-square)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-CI%2FCD-2088ff?style=flat-square&logo=github-actions&logoColor=white)

</div>

<br/>

## 📖 Documentación / Documentation

| Documento | Descripción |
|---|---|
| [`wiki/01-introduccion.md`](wiki/01-introduccion.md) | Introducción completa, cronograma, milestones y CI/CD |
| [`wiki/02-hardware-setup.md`](wiki/02-hardware-setup.md) | Conexiones físicas, pinout ESP32 y Raspberry Pi |
| [`wiki/03-sensor-calibration.md`](wiki/03-sensor-calibration.md) | Procedimientos de calibración sensor por sensor |
| [`wiki/04-architecture.md`](wiki/04-architecture.md) | Decisiones de arquitectura, esquemas protobuf, DB schema |
| [`wiki/05-deployment.md`](wiki/05-deployment.md) | Guía de despliegue LAN con Docker Compose |
| [`wiki/06-troubleshooting.md`](wiki/06-troubleshooting.md) | Errores comunes y soluciones |

<br/>

## 🤝 Contribuir / Contributing

1. Crea un fork del repositorio
2. Crea tu rama: `git checkout -b feature/nombre-descriptivo`
3. Haz commit siguiendo [Conventional Commits](https://www.conventionalcommits.org/): `feat(firmware): add EC sensor`
4. Abre un Pull Request hacia `develop`
5. El PR debe pasar todos los checks de CI antes de ser revisado

> Consulta las plantillas de issues en `.github/ISSUE_TEMPLATE/` para reportar bugs o proponer features.

<br/>

## 👥 Equipo / Team

| Rol | Responsabilidad |
|---|---|
| Hardware & Firmware | ESP32, sensores, calibración, C++ |
| Vision & RPi | YOLOv8, tracking, IR/NDVI, Python RPi |
| Backend & DevOps | FastAPI, PostgreSQL, RabbitMQ, CI/CD, Docker |

> Proyecto desarrollado en **CEATyCC Querétaro**, México · 2026

<br/>

## 📜 Licencia / License

Este proyecto está bajo la licencia **MIT**. Consulta el archivo [`LICENSE`](LICENSE) para más detalles.

---

<div align="center">

**ZenFlow** · Tecnológico Nacional de México - Instituto Tecnológico de Querétaro · 2026

*Cultivando bienestar, un ciclo a la vez / Growing wellbeing, one cycle at a time* 🌊🌿

</div>
