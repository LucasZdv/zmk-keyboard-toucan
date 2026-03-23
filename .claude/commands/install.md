# /install — Flashear Firmware al Toucan

## Description
Guía interactiva para flashear los archivos .uf2 al teclado Toucan, lado izquierdo y derecho.

## User-invocable
true

## Instructions

Ejecuta los siguientes pasos en orden:

### Paso 1: Verificar archivos
Verifica que existen los .uf2 en firmware/:
```bash
ls firmware/*.uf2
```
Si no existen, informa: "No hay firmware para flashear. Ejecuta `/build` primero." y detente.

### Paso 2: Flashear lado izquierdo
Pregunta al usuario con AskUserQuestion:
- "Conecta el **lado izquierdo** del Toucan por USB (cable de datos) y haz **doble-tap en RST**. ¿Ya está listo?"
- Opciones: "Listo, ya montó XIAO-BOOT" / "Necesito ayuda"

Cuando confirme:
1. Verifica que el volumen existe:
   ```bash
   ls /Volumes/ | grep -i xiao
   ```
2. Si existe, copia el firmware:
   ```bash
   cp firmware/toucan_left.uf2 /Volumes/XIAO-BOOT/
   ```
3. Espera 3 segundos y verifica que la unidad se desmontó (= éxito):
   ```bash
   ls /Volumes/ | grep -i xiao
   ```
4. Si la unidad desapareció: "Lado izquierdo flasheado con éxito."
5. Si sigue montada: "Hubo un problema. Intenta desconectar, reconectar y hacer doble-tap RST de nuevo."

El error `could not copy extended attributes` es normal y puede ignorarse — el firmware se copia correctamente.

### Paso 3: Flashear lado derecho
Pregunta al usuario con AskUserQuestion:
- "Ahora conecta el **lado derecho** del Toucan por USB y haz **doble-tap en RST**. ¿Ya está listo?"
- Opciones: "Listo, ya montó XIAO-BOOT" / "Necesito ayuda"

Cuando confirme:
1. Verifica que el volumen existe:
   ```bash
   ls /Volumes/ | grep -i xiao
   ```
2. Si existe, copia el firmware:
   ```bash
   cp firmware/toucan_right.uf2 /Volumes/XIAO-BOOT/
   ```
3. Espera 3 segundos y verifica que la unidad se desmontó:
   ```bash
   ls /Volumes/ | grep -i xiao
   ```
4. Si la unidad desapareció: "Lado derecho flasheado con éxito."

### Paso 4: Confirmación final
Informa al usuario:
"Ambos lados flasheados. Se reconectarán automáticamente por Bluetooth. Prueba tus teclas."

### Opción de ayuda
Si el usuario selecciona "Necesito ayuda":
- Explica que necesita un cable USB de **datos** (no solo carga)
- El **doble-tap en RST** debe ser rápido (como doble-clic)
- Debe aparecer una unidad "XIAO-BOOT" en Finder
- Si no aparece, probar con otro cable o puerto USB
