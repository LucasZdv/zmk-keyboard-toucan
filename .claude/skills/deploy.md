# /deploy — Commit, Push y Build del Firmware ZMK

## Description
Automatiza el proceso de commit, push y monitoreo del build de GitHub Actions para el firmware del Toucan.

## User-invocable
true

## Instructions

Ejecuta los siguientes pasos en orden:

### Paso 1: Verificar cambios
Ejecuta `git status` para verificar que hay cambios pendientes. Si no hay cambios, informa al usuario y detente.

### Paso 2: Mostrar cambios
Ejecuta `git diff` y `git diff --cached` para mostrar los cambios al usuario. Resume brevemente qué se modificó.

### Paso 3: Commit
- Ejecuta `git add` con los archivos modificados (nunca uses `git add -A` ni `git add .`)
- Crea un commit con un mensaje descriptivo y conciso sobre los cambios realizados
- Incluye `Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>` en el mensaje
- Usa HEREDOC para el mensaje del commit

### Paso 4: Push
Ejecuta `git push origin main`.

### Paso 5: Monitorear build
1. Espera 3 segundos para que GitHub registre el push
2. Obtén el run ID más reciente:
   ```bash
   gh run list -L 1 --json databaseId,status -q '.[0].databaseId'
   ```
3. Muestra el link al build: `https://github.com/LucasZdv/zmk-keyboard-toucan/actions`
4. Monitorea el build:
   ```bash
   gh run watch <RUN_ID> --exit-status
   ```
5. Informa el resultado:
   - Si exitoso: "Build completado. Usa `/build` para descargar los archivos."
   - Si falló: "Build falló. Revisa los logs." y ejecuta `gh run view <RUN_ID> --log-failed`
