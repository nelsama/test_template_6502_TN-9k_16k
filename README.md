# 6502 Template - Tang Nano 9K

🚀 **Proyecto base/template** para desarrollo en CPU 6502 sobre FPGA Tang Nano 9K.

Usa este proyecto como punto de partida para crear tus propias aplicaciones con el procesador 6502.

## Características

- ✅ CPU 6502 @ 3.375 MHz en FPGA Tang Nano 9K
- ✅ Control de 6 LEDs 
- ✅ Comunicación UART para debug
- ✅ Compilación con cc65
- ✅ **Compatible con librerías estándar de cc65** (stdlib, string, etc.)
- ✅ Startup con copydata y zerobss
- ✅ Estructura lista para expandir

## Hardware Soportado

| Componente | Dirección | Descripción |
|------------|-----------|-------------|
| PORT_SALIDA_LED | $C001 | Puerto de salida para 6 LEDs (bits 0-5) |
| CONF_PORT_SALIDA_LED | $C003 | Configuración: 0=salida, 1=entrada |
| UART | $C020-$C022 | Comunicación serial para debug |

## Estructura del Proyecto

```
├── src/
│   ├── main.c              # Programa principal (edita aquí)
│   ├── startup.s           # Inicialización del sistema
│   └── simple_vectors.s    # Vectores de interrupción 6502
├── libs/                   # Librerías (UART, Timer, I2C, etc.)
│   └── uart/
├── config/
│   └── fpga.cfg            # Configuración del linker cc65
├── scripts/
│   └── bin2rom3.py         # Conversor BIN → VHDL
├── build/                  # Archivos compilados (generado)
├── output/                 # ROM generada (generado)
└── makefile                # Compilación
```

## Cómo Usar este Template

1. **Clona o descarga** este repositorio
2. **Edita** `src/main.c` con tu código
3. **Agrega librerías** en la carpeta `libs/` según necesites
4. **Compila** con `make`
5. **Carga** `output/rom.vhd` en tu proyecto FPGA

## Compilación

### Requisitos previos
- [cc65](https://cc65.github.io/) instalado en `D:\cc65`
- Python 3 para el script de conversión

### Compilar
```bash
make
```

### Limpiar
```bash
make clean
```

### Cargar en FPGA
Copiar `output/rom.vhd` al proyecto FPGA y sintetizar.

## Uso de Librerías cc65

Este template incluye un startup que inicializa correctamente el runtime de cc65.
Puedes usar librerías estándar sin problemas:

```c
#include <stdlib.h>
#include <string.h>

int main(void) {
    char buffer[32];
    int random_num;
    
    srand(12345);
    random_num = rand() % 100;
    
    strcpy(buffer, "Hola 6502!");
    
    // ...
}
```

## Mapa de Memoria

| Región | Dirección | Tamaño | Descripción |
|--------|-----------|--------|-------------|
| Zero Page | $0002-$00FF | 254 bytes | Variables rápidas cc65 |
| RAM | $0100-$3DFF | ~15 KB | RAM principal + DATA |
| Stack | $3E00-$3FFF | 512 bytes | Pila del sistema |
| ROM | $8000-$9FF9 | 8 KB | Código del programa |
| Vectores | $9FFA-$9FFF | 6 bytes | NMI, RESET, IRQ |
| I/O | $C000-$C003 | 4 bytes | Puertos de E/S |
| UART | $C020-$C022 | 3 bytes | Comunicación serial |

## Archivos del Sistema

| Archivo | Descripción |
|---------|-------------|
| `startup.s` | Inicializa stack, copydata, zerobss y llama a main |
| `simple_vectors.s` | Define vectores NMI, RESET, IRQ |
| `fpga.cfg` | Mapa de memoria para el linker |

## Requisitos

- [cc65](https://cc65.github.io/) - Compilador C para 6502
- Python 3 - Para el script bin2rom3.py
- FPGA Tang Nano 9K

## Licencia

MIT
