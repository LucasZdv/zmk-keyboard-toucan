# /build — Descargar Firmware Compilado

## Description
Verifica si el build de GitHub Actions está listo, descarga los artifacts, los descomprime y renombra los .uf2 en la carpeta firmware/.

## User-invocable
true

## Instructions

Ejecuta los siguientes pasos en orden:

### Paso 1: Verificar estado del último build
```bash
gh run list -L 1 --json databaseId,status,conclusion,createdAt -q '.[0]'
```
- Si `status` es `in_progress` o `queued`: informa al usuario que el build aún no termina y espera con `gh run watch <RUN_ID> --exit-status`
- Si `conclusion` es `failure`: informa que el build falló y muestra logs con `gh run view <RUN_ID> --log-failed`. Detente.
- Si `conclusion` es `success`: continúa al paso 2.

### Paso 2: Limpiar carpeta firmware
```bash
rm -rf firmware/*.uf2
mkdir -p firmware
```

### Paso 3: Descargar artifacts
```bash
gh run download <RUN_ID> -D firmware/
```
Esto crea subcarpetas por cada artifact. Los .uf2 quedan dentro de subdirectorios.

### Paso 4: Mover y renombrar archivos
Mueve todos los .uf2 a la raíz de firmware/ con nombres limpios (sin espacios, usando guiones bajos):
```bash
find firmware/ -name "*.uf2" -exec mv {} firmware/ \;
```
Renombra los archivos para que no tengan espacios:
- `toucan_left_rgbled_adapter_nice_view_gem-seeeduino_xiao_ble-zmk.uf2` → `toucan_left.uf2`
- `toucan_right_rgbled_adapter-seeeduino_xiao_ble-zmk.uf2` → `toucan_right.uf2`
- `settings_reset-seeeduino_xiao_ble-zmk.uf2` → `settings_reset.uf2`

### Paso 5: Limpiar subdirectorios vacíos
```bash
find firmware/ -type d -empty -delete
```

### Paso 6: Confirmar
Lista los archivos finales con `ls -la firmware/*.uf2` e informa al usuario:
"Firmware listo. Usa `/install` para flashear el teclado."
