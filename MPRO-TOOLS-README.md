# Herramientas de Desarrollo MPro

Script para sincronizar ramas en los proyectos de MPro con procesamiento paralelo y gestión inteligente de dependencias.

## Script disponible

### mpro-sync - Sincronización de repositorios

Cambia de rama en todos los repositorios configurados con procesamiento paralelo para máxima velocidad.

**Uso básico:**

```bash
mpro-sync <nombre-rama> [opciones]
```

**Ejemplos:**

```bash
# Cambiar de rama sin instalar dependencias (por defecto)
mpro-sync develop
mpro-sync feature/888

# Cambiar de rama e instalar dependencias
mpro-sync develop -d
mpro-sync feature/nueva-funcionalidad --dependencies

# Ver estado actual sin cambiar de rama
mpro-sync status
mpro-sync --status

# Configurar repositorios
mpro-sync --configure

# Ver configuración actual
mpro-sync --show-config

# Instalar alias personalizado
mpro-sync --install

# Ver ayuda
mpro-sync --help
```

**Características principales:**

- Procesamiento paralelo (3x más rápido que ejecución secuencial)
- Coincidencia inteligente de patrones de rama (feature/888 encuentra feature/888-\*)
- Detección de ramas ambiguas (alerta si hay múltiples coincidencias)
- Instalación opcional de dependencias (por defecto: desactivada)
- Configuración personalizable por usuario
- Soporte para repositorios personalizados
- Mensajes de error detallados

**¿Qué hace?**

Por defecto (sin -d):

- Cambia a la rama especificada en todos los repos (en paralelo)
- Ejecuta git fetch y git pull en cada repositorio
- Muestra el estado final de todas las ramas

Con flag -d/--dependencies:

- Además de lo anterior, instala/actualiza dependencias:
  - Librería (lib-domain-patrimonial) en los APIs
  - Dependencias Python en los APIs (patrimonial y mpro-bff)
  - Dependencias npm en el frontend

**Coincidencia de patrones:**

El script busca coincidencias inteligentes:

- `develop` busca exactamente "develop"
- `feature/888` busca "feature/888", "feature/888-\*", etc.
- Si encuentra múltiples coincidencias, muestra todas las opciones y se detiene

## Configuración

### Primera vez

```bash
# Configurar repositorios
mpro-sync --configure

# Instalar alias (opcional)
mpro-sync --install
```

### Archivo de configuración

El archivo `~/.mpro-sync.conf` contiene:

- Rutas a los repositorios
- Orden de procesamiento
- Repositorios habilitados/deshabilitados

Ejemplo de personalización:

```bash
# Añadir repositorio personalizado
REPO_PATHS["mi-repo"]="/ruta/a/mi-repo"
REPO_ENABLED["mi-repo"]=true
REPO_ORDER=(lib-domain mi-repo api-patrimonial api-mpro-bff frontend)
```

## Estructura de repositorios

Repositorios por defecto:

- `appmutuactivostech-lib-domain-patrimonial/` - Librería compartida
- `appmutuactivostech-api-patrimonial/` - API Patrimonial
- `appmutuactivostech-api-mpro-bff/` - API BFF
- `appmutuactivostech-front-mpro/` - Frontend React

Se pueden añadir repositorios personalizados con `--configure`.

## Comandos de inicio rápido

Después de sincronizar, usar estos comandos para iniciar los servicios:

**API Patrimonial:**

```bash
cd /home/posan7m/mpro/appmutuactivostech-api-patrimonial
python app_dev.py
```

**API MPro BFF:**

```bash
cd /home/posan7m/mpro/appmutuactivostech-api-mpro-bff
python app_dev.py
```

**Frontend:**

```bash
cd /home/posan7m/mpro/appmutuactivostech-front-mpro
npm run dev
```

El script muestra estos comandos al finalizar la sincronización.

## Solución de problemas

**Error: rama no encontrada**

El script busca coincidencias parciales. Verificar ramas disponibles:

```bash
cd /ruta/al/repo
git branch -a | grep <patron>
```

**Error: múltiples ramas coinciden**

El script muestra todas las ramas encontradas. Usar un patrón más específico:

```bash
# En lugar de:
mpro-sync feature/888

# Usar el nombre completo:
mpro-sync feature/888-gestion-test
```

**Error: instalación de dependencias falla**

Ejecutar manualmente para ver detalles:

```bash
# Para APIs Python
cd /ruta/al/api
pip install -r requirements.txt

# Para frontend
cd /ruta/al/frontend
npm install
```

**Verificar configuración**

```bash
mpro-sync --show-config
```

**Reconfigurar repositorios**

```bash
mpro-sync --configure
```

## Rendimiento

Tiempos típicos de ejecución (4 repositorios):

- Sin dependencias: ~2.5 segundos
- Con dependencias (-d): ~30-60 segundos (depende del estado de las dependencias)
