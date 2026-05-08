# 🎮 PokeDex — Despliegue en Azure Static Web Apps

Aplicación web desarrollada en **Angular** que permite explorar las 151 especies de Pokémon de la primera generación, mostrando información detallada sobre sus características y habilidades.

---

## 🔗 URL Pública

👉 [https://gentle-dune-0b529080f.7.azurestaticapps.net](https://gentle-dune-0b529080f.7.azurestaticapps.net)

---

## ☁️ Plataforma de Despliegue

**Azure Static Web Apps** — Microsoft Azure for Students

| Parámetro | Valor |
|---|---|
| Suscripción | Azure for Students |
| Grupo de recursos | pokemon |
| Tipo de plan | Gratuito |
| Región | East US 2 |
| Origen del código | GitHub |
| Rama | main |
| Ubicación de salida | dist/pokedex-angular |

---

## 📁 Repositorio GitHub

👉 [https://github.com/Gersonjim/pokedex](https://github.com/Gersonjim/pokedex)

---

## 🛠️ Tecnologías Usadas

- **Angular** — Framework frontend
- **TypeScript** — Lenguaje de programación
- **PokeAPI** — API REST para datos de Pokémon
- **Azure Static Web Apps** — Plataforma de despliegue
- **GitHub Actions** — CI/CD automatizado
- **GitHub Desktop** — Control de versiones

---

## 🔒 Seguridad

La aplicación implementa los siguientes encabezados HTTP de seguridad configurados en `staticwebapp.config.json`:

| Header | Propósito |
|---|---|
| `Content-Security-Policy` | Previene ataques XSS controlando qué recursos puede cargar la app |
| `Strict-Transport-Security` | Obliga el uso de HTTPS durante 1 año |
| `X-Content-Type-Options` | Evita la detección automática de tipos de archivo |
| `X-Frame-Options` | Previene ataques de clickjacking mediante iframes |
| `Referrer-Policy` | Minimiza la fuga de información en cabeceras HTTP |
| `Permissions-Policy` | Deshabilita acceso a cámara, micrófono y GPS |

### Resultados de Escaneo de Seguridad

- 🔐 **securityheaders.com**: Calificación **A**
- 🔒 **SSL Labs**: Calificación **A+**

---

## 🚀 Cómo Crear la Cuenta en Azure for Students

1. Ingresar a [azure.microsoft.com/es-es/free/students](https://azure.microsoft.com/es-es/free/students/)
2. Hacer clic en **"Activar ahora"**
3. Iniciar sesión con el correo institucional universitario
4. Completar la verificación de estudiante
5. Aceptar los términos y condiciones
6. La cuenta se activa con **$100 USD de crédito gratuito**

---

## 📋 Proceso de Despliegue

Ver el archivo [Despliegue.md](./Despliegue.md) para el proceso detallado con comandos y configuraciones.

---

## 🧠 Reflexión Técnica

### ¿Qué vulnerabilidades previenen los encabezados implementados?

- **CSP** previene ataques XSS al controlar qué scripts y recursos se pueden cargar
- **HSTS** previene ataques de downgrade forzando siempre HTTPS
- **X-Frame-Options** previene clickjacking bloqueando iframes externos
- **X-Content-Type-Options** previene la ejecución de archivos maliciosos disfrazados

### ¿Qué aprendí sobre la relación entre despliegue y seguridad web?

Aprendí que desplegar una aplicación sin considerar los encabezados HTTP de seguridad deja la app vulnerable a múltiples vectores de ataque. Azure Static Web Apps facilita la configuración de estos headers mediante el archivo `staticwebapp.config.json`, integrando seguridad desde el proceso de despliegue.

### ¿Qué desafíos encontré?

- Configurar correctamente el `Content-Security-Policy` para Angular, que requiere `unsafe-inline` y `unsafe-eval`
- Resolver el error de política de Azure al seleccionar la región correcta
- Sincronizar los commits locales con los cambios automáticos de Azure (Fetch origin)
- Ajustar los dominios permitidos en el CSP para que las imágenes de Pokémon cargaran correctamente

---

*Desarrollado por Gersonjim — Pueblo Paleta Inc. / PumasLab*
