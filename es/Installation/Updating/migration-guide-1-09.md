---
order: 109
route: /installation/updating/migration-guide-1-09/
---

# Guía de Migración 1.9.0

## ¿Cómo migrar a una nueva rama si uso main/dev?

_**Se recomienda realizar una instalación nueva.**_ Sin embargo, si desea usar una copia existente de SillyTavern, siga las instrucciones a continuación.

**¡IMPORTANTE!** Antes de hacer nada, haga *una copia de seguridad completa* de su instalación. Puede *perder sus datos* en el proceso, así que no ignore esta advertencia.

¿No está seguro de qué archivos respaldar? Vea la lista aquí: [Cómo actualizar SillyTavern](/Installation/Updating/index.md#updating-from-1120-to-1120)

### Instalaciones con git

1. Abra un terminal (cmd, PowerShell, Termux, etc.) en su carpeta de instalación de SillyTavern.
2. Escriba `git fetch` y luego `git pull` para obtener las actualizaciones.
3. Puede perder su configuración. ¿Ha hecho una copia de seguridad? `git switch release` o `git switch staging` cambiarán su rama, respectivamente
4. Salte al siguiente elemento si no tiene errores. Puede tener algo como:
   ```
   error: Your local changes to the following files would be overwritten by checkout:
        config.conf
        public/css/bg_load.css
        public/settings.json
   ```
   Verá una lista de archivos afectados. Si no le importa que esos archivos de configuración se reemplacen, `git switch -f release` o `git switch -f staging` establecerán su rama. Si le importa guardar esos cambios, restaure desde la copia de seguridad.

5. Escriba `npm install` y luego `npm run start` para probar que todo funcione correctamente.
6. ¡Disfrute! Restaure sus datos desde una copia de seguridad si es necesario.

### fatal: invalid reference: release

Esto puede suceder si clonó solo una rama de un repositorio remoto antiguo (antes de la migración al repositorio de la organización). Para solucionar esto, necesita agregar y obtener una rama de un nuevo repositorio remoto:

```
git remote add st https://github.com/SillyTavern/SillyTavern
git fetch st
git checkout -t st/release
```

Luego proceda desde el paso 5.

### Instalaciones con ZIP

Nada cambia para usted. Solo descargue el ZIP de rama/release como de costumbre.
