# UK101ESP32 - Emulador de Compukit UK101 para ESP32

## 📖 Descripción

**UK101ESP32** es un emulador completo del **Compukit UK101** original, portado para ejecutarse en un **ESP32**.

El emulador incluye:
- ✅ CPU 6502 completo con timing preciso
- ✅ Monitor CEGMON
- ✅ Microsoft BASIC
- ✅ Interfaz serie a 115200 baud
- ✅ Compatibilidad con minicom y otros terminales

Más información en Minibots: [https://minibots.wordpress.com/2025/09/29/emulador-de-compukit-uk101-con-esp32]

## 🏗️ Arquitectura

### Componentes Principales

1. **CPU 6502** (`cpu6502.cpp/h`)
   - Emulación ciclo-precisa del microprocesador 6502
   - Ejecuta a 1.000 MHz (velocidad original del UK101)

2. **MC6850 ACIA** (`mc6821.cpp/h`)
   - Interfaz de E/S
   - Gestiona comunicación teclado/pantalla

3. **Motherboard** (`motherboard.cpp/h`)
   - Mapeo de memoria (32KB RAM + 32KB ROM)
   - Decodificación de direcciones

4. **Terminal** (`terminal.cpp/h`)
   - Entrada serie desde ESP32
   - Salida formateada para terminales modernos

5. **ROM Embebida** (`uk101.rom.h`)
   - Monitor GECMON + BASIC
   - Datos embebidos en memoria flash

## 🔄 Cambios desde la Versión Original (PC)

### Adaptaciones para ESP32

| Aspecto | Versión PC | Versión ESP32 |
|---------|------------|----------------|
| **E/S** | Terminal nativo | Comunicación serie |
| **ROM** | Archivo externo | Datos embebidos |
| **Timing** | `clock_gettime()` | `micros()` |
| **Arquitectura** | `main()` loop | `setup()`/`loop()` Arduino |
| **Memoria** | Asignación dinámica | Arrays estáticos |
| **Dependencias** | POSIX threads | FreeRTOS |

### Mejoras Implementadas

- **Buffer de teclado**: Lógica de interrupciones del MC6821
- **Timing preciso**: Ciclos exactos para 1.000 MHz
- **Sin dependencias externas**: Todo embebido en el ESP32

## 🚀 Instalación y Uso

### Requisitos

- **Hardware**: ESP32 (cualquier modelo)
- **Software**:
  - Arduino IDE con soporte ESP32
  - Terminal serie (minicom, screen, PuTTY, etc.)
- **Conexión**: USB a serie (FTDI, CP2102, etc.)

### Compilación

1. **Instalar Arduino IDE** con soporte ESP32:
   ```bash
   # En Linux, instalar desde repositorio o descargar
   sudo apt install arduino
   ```

2. **Agregar board ESP32**:
   - En Arduino IDE: Archivo → Preferencias
   - URL: `https://dl.espressif.com/dl/package_esp32_index.json`
   - Herramientas → Placa → ESP32 Arduino → ESP32 Dev Module

3. **Compilar y subir**:
   ```bash
   cd airesp32
   arduino-cli compile --fqbn esp32:esp32:esp32 uk101resp32.ino
   arduino-cli upload --fqbn esp32:esp32:esp32 --port /dev/ttyUSB0 uk101resp32.ino
   ```

### Configuración del Terminal

#### Usando minicom:
```bash
# Instalar minicom
sudo apt install minicom

# Conectar al ESP32
minicom -D /dev/ttyUSB0 -b 115200

# Configuración (Ctrl+A → O):
# Serial port setup → 115200 8N1
# Hardware flow control → No
# Software flow control → No
```

#### Usando screen:
```bash
screen /dev/ttyUSB0 115200
```

## 🎮 Uso del Emulador

### Arranque

Al conectar, verás:
```
=================================
UK101 Computer Emulator for ESP32
   Based on UK101RE by ea4eoz
=================================


### Características Especiales

- **Líneas de 40 caracteres**: Auto-wrap como el Apple I original
- **Timing preciso**: 1.023 MHz de emulación exacta
- **Interrupciones**: Manejo correcto de teclado

## 📊 Especificaciones Técnicas

- **CPU**: 6502 emulado a 1.023 MHz
- **RAM**: 32 KB (direcciones 0x0000-0x7FFF)
- **ROM**: 8 KB (direcciones 0xE000-0xFFFF)
- **E/S**: MC6821 PIA en 0xD000-0xDFFF
- **Baud rate**: 115200 (8N1)
- **Consumo**: ~70KB RAM, ~312KB Flash

## 🐛 Solución de Problemas

### El ESP32 no responde
- Verificar conexión USB
- Comprobar puerto serie: `ls /dev/ttyUSB*`
- Reiniciar ESP32 (botón EN)

### Caracteres extraños en terminal
- Verificar baud rate: 115200
- Configurar terminal: 8N1, sin flow control
- Probar con otro programa terminal

### Backspace no funciona
- Es normal: el Apple I no tenía edición de línea
- CTRL+H envía el carácter de backspace al programa


## 📁 Estructura del Proyecto

```
uk101esp32/
├── uk101esp32.ino          # Sketch principal Arduino
├── cpu6502.cpp/h           # Emulador CPU 6502
├── mc6850.cpp/h            # PIA MC6821
├── motherboard.cpp/h       # Mapeo de memoria
├── terminal.cpp/h        # Interfaz serie
├── uk101.rom.h            # ROM embebida
└── README.md             # Esta documentación
```

## 🤝 Contribuir

1. Fork el proyecto
2. Crear rama para feature (`git checkout -b feature/nueva-funcion`)
3. Commit cambios (`git commit -am 'Agrega nueva función'`)
4. Push a rama (`git push origin feature/nueva-funcion`)
5. Crear Pull Request

## 📄 Licencia

Este proyecto está bajo **GNU Lesser General Public License v3.0**.

Basado en AIRE/UK101RE.

## 🙏 Agradecimientos

- **Miguel A. Vallejo**: Creador del emulador AIRE/UK101RE original
- **Comunidad ESP32**: Por hacer posible este port

## 🔗 Enlaces

- [Repositorio original UK101RE](https://github.com/ea4eoz/uk101re)

---

**¡Disfruta reviviendo la historia de la computación!** 🖥️
