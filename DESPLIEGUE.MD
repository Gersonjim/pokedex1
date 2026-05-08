# 📦 Despliegue.md — Proceso Detallado de Despliegue PokeDex en Azure

**Estudiante:** Gersonjim  
**Plataforma:** Azure Static Web Apps  
**Fecha:** mayo 2026  
**Repositorio:** [https://github.com/Gersonjim/pokedex](https://github.com/Gersonjim/pokedex)  
**URL Pública:** [https://gentle-dune-0b529080f.7.azurestaticapps.net](https://gentle-dune-0b529080f.7.azurestaticapps.net)

---

## 1. Pre-requisitos

- Cuenta en **GitHub**
- Cuenta en **Azure for Students**
- **GitHub Desktop** instalado
- **Visual Studio Code** instalado
- Código fuente de la PokeDex (Angular)

---

## 2. Creación del Repositorio en GitHub

1. Ingresar a [github.com](https://github.com)
2. Clic en **"New repository"**
3. Configurar:
   - Nombre: `pokedex`
   - Visibilidad: **Public**
   - Marcar **"Add README"**
4. Clic en **"Create repository"**

---

## 3. Clonar el Repositorio con GitHub Desktop

1. Abrir **GitHub Desktop**
2. Ir a **File → Clone repository**
3. Seleccionar el repositorio `pokedex`
4. Elegir la ruta local de destino
5. Clic en **"Clone"**
6. Clic en **"Show in Explorer"** para abrir la carpeta

---

## 4. Subir el Código Fuente

1. Descomprimir el archivo ZIP de la PokeDex dentro de la carpeta clonada
2. Verificar que los archivos queden en la raíz (no en subcarpeta):

```
📁 pokedex/
  ├── 📁 src/
  ├── 📁 .github/
  ├── 📄 angular.json
  ├── 📄 package.json
  └── ...
```

3. En **GitHub Desktop**:
   - Summary: `Agregar código fuente PokeDex Angular`
   - Clic en **"Commit to main"**
   - Clic en **"Push origin"**

---

## 5. Configuración del Proyecto en VS Code

Abrir VS Code desde GitHub Desktop → **"Open in Visual Studio Code"**

### 5.1 Editar environment.prod.ts

Ruta: `src/environments/environment.prod.ts`

```typescript
// ❌ Antes
imagesPath: '/pokedex-angular/assets/images',

// ✅ Después
imagesPath: '/assets/images',
```

Guardar con **Ctrl + S**

### 5.2 Verificar environment.ts

Ruta: `src/environments/environment.ts`

Verificar que también tenga:
```typescript
imagesPath: '/assets/images',
```

### 5.3 Eliminar pipeline conflictivo

Eliminar el archivo: `.github/workflows/pipelines.yml`

Este archivo causaba conflictos con el pipeline que genera Azure automáticamente.

### Commit de los cambios

En **GitHub Desktop**:
- Summary: `Corregir ruta de imágenes y eliminar pipeline conflictivo`
- Clic en **"Commit to main"**
- Clic en **"Push origin"**

---

## 6. Configuración de Headers de Seguridad

Crear el archivo `staticwebapp.config.json` en la raíz del proyecto:

```json
{
  "globalHeaders": {
    "Content-Security-Policy": "default-src 'self'; script-src 'self' 'unsafe-eval' 'unsafe-inline'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com https://cdn.jsdelivr.net; style-src-elem 'self' 'unsafe-inline' https://fonts.googleapis.com https://cdn.jsdelivr.net; img-src 'self' data: https://raw.githubusercontent.com https://pokeapi.co https://assets.pokemon.com; connect-src 'self' https://pokeapi.co https://beta.pokeapi.co https://graphql-pokeapi.vercel.app; font-src 'self' https://fonts.gstatic.com https://cdn.jsdelivr.net;",
    "Strict-Transport-Security": "max-age=31536000; includeSubDomains; preload",
    "X-Content-Type-Options": "nosniff",
    "X-Frame-Options": "DENY",
    "Referrer-Policy": "no-referrer",
    "Permissions-Policy": "geolocation=(), microphone=(), camera=()"
  }
}
```

En **GitHub Desktop**:
- Summary: `Agregar headers de seguridad HTTP`
- Clic en **"Commit to main"**
- Clic en **"Push origin"**

---

## 7. Despliegue en Azure Static Web Apps

1. Ingresar a [portal.azure.com](https://portal.azure.com)
2. Buscar **"Static Web Apps"** → Clic en **"+ Crear"**
3. Completar el formulario:

| Campo | Valor |
|---|---|
| Suscripción | Azure for Students |
| Grupo de recursos | pokemon (nuevo) |
| Nombre | pokedex |
| Tipo de plan | Gratis |
| Origen | GitHub |
| Organización | Gersonjim |
| Repositorio | pokedex |
| Rama | main |
| Ubicación de la app | / |
| Ubicación de la API | (vacío) |
| Ubicación de salida | dist/pokedex-angular |

4. Clic en **"Revisar y crear"** → **"Crear"**

### ⚠️ Error encontrado y solución

**Error:** `The template deployment failed because of policy violation`

**Causa:** La región seleccionada estaba bloqueada para cuentas Azure for Students.

**Solución:** Cambiar la región a **East US 2** y volver a crear.

---

## 8. Verificación del Despliegue

1. Clic en **"Ir al recurso"**
2. Clic en **"Visitar el sitio"**
3. Verificar que la PokeDex carga con todos los Pokémon ✅

### ⚠️ Error de CSP encontrado y solución

**Error en consola del navegador:**
```
Loading the image 'https://assets.pokemon.com/...' violates Content Security Policy
```

**Causa:** El dominio `assets.pokemon.com` no estaba en la lista blanca del CSP.

**Solución:** Actualizar `staticwebapp.config.json` agregando `https://assets.pokemon.com` al `img-src`.

---

## 9. Auditoría de Seguridad

### 9.1 securityheaders.com

- **URL escaneada:** https://gentle-dune-0b529080f.7.azurestaticapps.net
- **Resultado:** ⭐ **A**
- **Nota:** El CSP contiene `unsafe-inline` y `unsafe-eval` requeridos por Angular

### 9.2 SSL Labs

- **URL escaneada:** https://gentle-dune-0b529080f.7.azurestaticapps.net
- **Resultado:** 🏆 **A+**
- Azure Static Web Apps incluye certificados TLS de alta calidad automáticamente

---

## 10. Historial de Commits

| # | Mensaje | Descripción |
|---|---|---|
| 1 | `Agregar código fuente PokeDex Angular` | Subida inicial del código |
| 2 | `Corregir ruta de imágenes y eliminar pipeline conflictivo` | Fix de environments y pipeline |
| 3 | `Agregar headers de seguridad HTTP` | Configuración CSP y headers |
| 4 | `Corregir CSP para permitir estilos e imágenes de Pokemon` | Fix dominios en CSP |
| 5 | `Agregar README.md y Despliegue.md` | Documentación del proyecto |

---

## 11. Variables de Entorno Configuradas en Azure

En Azure Static Web Apps → **Configuración de la aplicación**:

| Variable | Valor |
|---|---|
| Ubicación de la app | `/` |
| Ubicación de salida | `dist/pokedex-angular` |
| Rama de despliegue | `main` |

---

*Documento generado para el caso de estudio — Pueblo Paleta Inc. / PumasLab*
