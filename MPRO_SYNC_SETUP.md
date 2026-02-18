# Guía de Instalación de MPro Sync

## Configuración Rápida para el Equipo

### 1. Configuración Inicial

```bash
# Hacer el script ejecutable (si no lo es ya)
chmod +x /ruta/a/mpro-sync

# O dejar que el instalador lo haga:
/ruta/a/mpro-sync --install
```

### 2. Configurar Repositorios

```bash
mpro-sync --configure
```

Esto hará:
- Solicitar el directorio base donde están los repositorios MPro
- Listar todos los repositorios a configurar
- Permitir añadir repositorios personalizados
- Mostrar qué repositorios están activados/desactivados

### 3. Crear un Alias (Opcional pero Recomendado)

```bash
mpro-sync --install
```

El instalador:
- Hará el script ejecutable automáticamente
- Preguntará el nombre del alias deseado (por defecto: `mpro-sync`)
- Lo añadirá a la configuración del shell (~/.bashrc, ~/.zshrc, o ~/.bash_profile)

Puedes usar cualquier nombre que prefieras:
- `my-mpro`
- `sync`
- `branch-sync`
- etc.

### 4. Comenzar a Usar

```bash
# Ver ramas actuales
mpro-sync status

# Cambiar de rama
mpro-sync develop
mpro-sync feature/888

# Con instalación de dependencias
mpro-sync develop -d

# Ver configuración
mpro-sync --show-config

# Obtener ayuda en cualquier momento
mpro-sync --help
```

## Comandos Comunes

```bash
# Comprobar estado (sin realizar cambios)
mpro-sync status
mpro-sync --status

# Cambiar a una rama (con coincidencia de patrones)
mpro-sync develop              # Coincidencia exacta
mpro-sync feature/888          # Encuentra ramas feature/888-*
mpro-sync fix/123 -d           # Con dependencias

# Reconfigurar más tarde
mpro-sync --configure          # Actualizar configuración
mpro-sync --show-config        # Ver configuración actual

# Compartir con otros
# Solo enviar el archivo del script y ejecutar la configuración
```

## Archivo de Configuración

Después de la configuración inicial, editar `~/.mpro-sync.conf` para:
- Añadir repositorios personalizados
- Activar/desactivar repositorios específicos
- Cambiar rutas de repositorios
- Modificar el orden de sincronización

Ejemplo:
```bash
# Añadir un repositorio personalizado
REPO_PATHS["my-custom-lib"]="/ruta/a/custom/lib"
REPO_ENABLED["my-custom-lib"]=true
REPO_ORDER=(lib-domain my-custom-lib api-patrimonial api-mpro-bff frontend)
```

## Permisos

El comando `--install` establece automáticamente el script como ejecutable:
```bash
chmod +x /ruta/a/mpro-sync
```

Si es necesario, puedes hacerlo manualmente antes de la instalación.

## Notas

- La configuración se almacena en `~/.mpro-sync.conf` (una por usuario)
- Cada miembro del equipo puede tener rutas y aliases diferentes
- El script funciona con cualquier estructura de repositorios git
- Se pueden añadir repositorios personalizados con `--configure`

## Características

- Coincidencia inteligente de patrones de rama (encuentra ramas `feature/888-*`)
- Instalación opcional de dependencias
- Mostrar estado actual sin realizar cambios
- Soporte para repositorios personalizados
- Salida colorizada
- Aliases configurables
- Funciona con bash, zsh y bash_profile
- Procesamiento paralelo para mayor velocidad
- Detección de ramas ambiguas
- Mensajes de error detallados

---

Creado por poses
