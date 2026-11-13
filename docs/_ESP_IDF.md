# Migración inicial a ESP-IDF

## Objetivo

Desarrollar un backend alternativo basado en ESP-IDF para compilar proyectos destinados a placas ESP32 mediante `idf.py build`.

Esta migración busca utilizar las herramientas nativas de ESP-IDF en lugar de depender de Arduino IDE o Arduino CLI para este flujo de compilación.

El repositorio conserva también una implementación basada en Arduino CLI (`src/arduino_backend.py`), por lo que la existencia de esta migración no implica que dicho backend haya sido eliminado.

## Consideraciones técnicas

- El ejemplo `hello_idf` es un proyecto ESP-IDF escrito en C, no un sketch `.ino`.
- El backend Python funciona como adaptador para ejecutar las herramientas de ESP-IDF; no implementa un compilador propio.
- Se requiere tener ESP-IDF y su toolchain instalados.
- El backend prepara automáticamente el entorno mediante `export.bat`, por lo que no es necesario activar manualmente una terminal ESP-IDF antes de ejecutarlo.

## Validación realizada en Windows

El 12 de noviembre de 2026 se ejecutó el backend desde una terminal PowerShell normal utilizando ESP-IDF 6.1.

Se compiló el proyecto de ejemplo `hello_idf` y se obtuvo:

- `success: true`
- `returncode: 0`
- Salida de error vacía.
- Tres archivos BIN identificados: `bootloader.bin`, `hello_idf.bin` y `partition-table.bin`.

La evidencia de esta ejecución se conserva en:

`evidence/compilacion_hello_idf_2026-11-12.json`

También se conserva el diagnóstico del problema inicial de selección del entorno Python:

`evidence/diagnostico_entorno_python_2026-11-12.json`

## Ejecución del backend

Desde la raíz del repositorio, ejecutar:

```powershell
python .\src\esp_idf_backend.py "C:\ruta\al\proyecto"
```

Para especificar el objetivo ESP32:

```powershell
python .\src\esp_idf_backend.py "C:\ruta\al\proyecto" --target esp32
```

El proyecto debe ser válido para ESP-IDF y contener un archivo `CMakeLists.txt` en su directorio raíz.

## Alcance y limitaciones

La validación realizada demuestra la compilación del proyecto de ejemplo mediante el backend ESP-IDF y la identificación de sus archivos binarios.

No demuestra:

- La carga del firmware en una placa física.
- La integración con la interfaz Sketch.
- La comunicación con los módulos Editor/Compilador y Log.
- La compatibilidad validada con todos los objetivos ESP32.
- La gestión de bibliotecas externas.

Estas funcionalidades requieren pruebas adicionales.