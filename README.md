<div align="center">

# 🌐 MikroTik Hotspot Portal - Caldero Sur WiFi

[![RouterOS Version](https://img.shields.io/badge/MikroTik-RouterOS%20v6%20%7C%20v7-004d80?style=for-the-badge&logo=mikrotik&logoColor=white)](https://mikrotik.com)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/es/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/es/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/es/docs/Web/JavaScript)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

*Plantilla moderna, optimizada, completamente responsiva y en español para portales cautivos de **MikroTik RouterOS**, diseñada para la red de clientes de **WiFi Pasapalos - Caldero Sur**.*

<br>

![Vista Previa del Portal Cautivo](LoginRecurso.png)

</div>

---

## 📋 Tabla de Contenidos
- [Características Principales](#-características-principales)
- [Arquitectura y Archivos del Proyecto](#-arquitectura-y-archivos-del-proyecto)
- [Variables de RouterOS Soportadas](#-variables-de-routeros-soportadas)
- [Guía de Instalación en MikroTik](#-guía-de-instalación-en-mikrotik)
  - [Método 1: Vía Winbox (Recomendado)](#método-1-vía-winbox-recomendado)
  - [Método 2: Vía FTP](#método-2-vía-ftp)
  - [Método 3: Configuración por Terminal (CLI)](#método-3-configuración-por-terminal-cli)
- [Configuración del Perfil Hotspot en MikroTik](#-configuración-del-perfil-hotspot-en-mikrotik)
- [Personalización y Diseño](#-personalización-y-diseño)
- [Resolución de Problemas Frecuentes (FAQ)](#-resolución-de-problemas-frecuentes-faq)
- [Licencia](#-licencia)

---

## 🚀 Características Principales

- 📱 **Diseño 100% Responsivo:** Adaptado a pantallas de teléfonos inteligentes, tablets y ordenadores portátiles.
- 🔐 **Autenticación CHAP Segura:** Incluye la librería criptográfica `md5.js` requerida para procesar el desafío CHAP (`$(chap-challenge)`) sin enviar contraseñas en texto plano por la red inalámbrica.
- ⏱️ **Modo Prueba Gratuita (Trial):** Botón integrado con lógica condicional `$(if trial == 'yes')` para permitir navegación temporal a nuevos usuarios.
- 🛡️ **Manejo de Errores Traducido:** Interceptación por JavaScript de los mensajes internos de RouterOS en inglés a mensajes amigables en español:
  - `invalid user or password` ➔ *Usuario o contraseña incorrectos.*
  - `stored counters reached` ➔ *Se ha agotado su tiempo o saldo.*
  - `no more sessions are allowed` ➔ *Ya tienes una sesión activa.*
- 📊 **Panel de Estado de Sesión (`status.html`):** Consulta en tiempo real de tiempo consumido (`$(uptime)`), tiempo restante, datos subidos/descargados (`$(bytes-in-nice)` / `$(bytes-out-nice)`) y botón de desconexión.
- ⚡ **Carga Ultraligera:** Sin dependencias externas pesadas como jQuery o frameworks inflados; HTML semántico y CSS puro para máxima velocidad de apertura en portales cautivos.

---

## 📁 Arquitectura y Archivos del Proyecto

```text
html-pagina-microtik/
├── login.html              # Página principal del portal cautivo (Formulario de login)
├── login.css               # Estilos globales, paleta de colores y componentes visuales
├── md5.js                  # Algoritmo de hash MD5 para el reto de autenticación CHAP
├── status.html             # Panel de estado mientras el usuario navega conectado
├── logout.html             # Confirmación de cierre de sesión con botón para reconectar
├── alogin.html             # Página puente de redirección post-autenticación
├── error.html              # Pantalla para errores críticos o accesos no autorizados
├── Caldero-Sur-Wifi.png    # Logotipo corporativo de Caldero Sur
├── usuario.svg             # Ícono vectorial del campo de usuario
├── candado.svg             # Ícono vectorial del campo de contraseña
├── Fondo.jpg               # Imagen de fondo para el fondo del portal
├── textura.jpg             # Textura gráfica de la cabecera del card
├── LoginRecurso.png        # Recursos gráficos adicionales
└── README.md               # Documentación técnica del proyecto
```

---

## 🧩 Variables de RouterOS Soportadas

Esta plantilla implementa las etiquetas de reemplazo nativas de MikroTik RouterOS (respetando la nomenclatura sensible a mayúsculas/minúsculas):

| Variable MikroTik | Propósito |
| :--- | :--- |
| `$(link-login-only)` | URL de acción para enviar el formulario de autenticación POST. |
| `$(link-orig)` | URL original que solicitó el cliente antes de ser interceptado. |
| `$(link-redirect)` | URL a la cual redirigir al usuario tras login exitoso. |
| `$(link-logout)` | URL para enviar la petición de cierre de sesión activa. |
| `$(link-status)` | URL del popup/página de estado de conexión. |
| `$(chap-id)` / `$(chap-challenge)` | Valores para el cálculo del hash MD5 en autenticación CHAP. |
| `$(error)` | Mensaje de error generado por el sistema de Hotspot. |
| `$(username)` | Nombre de usuario ingresado o activo en la sesión. |
| `$(ip)` / `$(mac)` | Dirección IP y MAC asignadas al cliente conectado. |
| `$(uptime)` | Tiempo transcurrido de navegación continua. |
| `$(session-time-left)` | Tiempo restante de la ficha/voucher del usuario. |
| `$(bytes-in-nice)` / `$(bytes-out-nice)` | Tráfico de subida y bajada en formato legible (KB, MB, GB). |

---

## 🛠️ Guía de Instalación en MikroTik

### Método 1: Vía Winbox (Recomendado)

1. Abra **Winbox** y conéctese a su router MikroTik.
2. En el menú lateral izquierdo, haga clic en **Files**.
3. Localice o cree la carpeta del Hotspot (generalmente llamada `hotspot` o `flash/hotspot` en routers con memoria Flash como RB750r2/hAP/Hex).
4. Seleccione todos los archivos de esta carpeta local (`login.html`, `login.css`, `md5.js`, `status.html`, etc.) y **arrástrelos y suéltelos** dentro de la carpeta `hotspot` en Winbox.
5. Verifique que todos los archivos se hayan transferido correctamente.

### Método 2: Vía FTP

Si tiene el servicio FTP habilitado en MikroTik (`/ip service enable ftp`):

```bash
# Conéctese usando su cliente FTP favorito (FileZilla o línea de comandos)
ftp 192.168.88.1
# Ingrese su usuario y contraseña de RouterOS
# Navegue a la carpeta destino:
cd hotspot
# Suba todos los archivos en modo binario
mput *.*
bye
```

### Método 3: Configuración por Terminal (CLI)

Para asociar la carpeta de archivos a su servidor de Hotspot y definir el método de autenticación:

```routeros
# 1. Asignar el directorio HTML al perfil de Hotspot
/ip hotspot profile set [find name="hsprof1"] html-directory=hotspot

# 2. Habilitar autenticación HTTP-CHAP y HTTP-PAP
/ip hotspot profile set [find name="hsprof1"] login-by=http-chap,http-pap

# 3. (Opcional) Si desea habilitar la prueba gratis (Trial)
/ip hotspot profile set [find name="hsprof1"] trial-uptime-limit=00:30:00 trial-uptime-reset=1d
/ip hotspot profile set [find name="hsprof1"] login-by=http-chap,http-pap,trial
```

---

## ⚙️ Configuración del Perfil Hotspot en MikroTik

Para que la plantilla funcione al 100% de sus capacidades, asegúrese de revisar en `/ip hotspot profile`:

1. **HTML Directory:** Debe apuntar exactamente al directorio donde subió los archivos (por ejemplo `hotspot` o `flash/hotspot`).
2. **Login By:** Marque las casillas **HTTP CHAP** y **HTTP PAP**. El algoritmo `md5.js` provisto protegerá el envío de las credenciales con CHAP.
3. **HTTP Cookie:** Si desea que los usuarios no tengan que reingresar credenciales si se desconectan brevemente, active **Cookie** con un tiempo de vida prudente (ejemplo: 3 días).

---

## 🎨 Personalización y Diseño

### Cambiar Colores Corporativos
En [`login.css`](file:///c:/Users/Pagina-web1/Videos/html-pagina-microtik/login.css), puede modificar los colores principales buscando los siguientes selectores:
- **Color de Acento / Botones (Naranja):**
  ```css
  .btn-login {
      background-color: #e67e22; /* Color principal */
  }
  .btn-login:hover {
      background-color: #d35400; /* Hover */
  }
  ```
- **Fondo de Pantalla:**
  Reemplace `Fondo.jpg` o edite la regla `body` en `login.css` para apuntar a otra imagen.
- **Logotipo:**
  Reemplace el archivo `Caldero-Sur-Wifi.png` con su logotipo en formato PNG con fondo transparente manteniendo el mismo nombre o actualizando la etiqueta `<img>` en los archivos HTML.

---

## ❓ Resolución de Problemas Frecuentes (FAQ)

### 1. ¿Por qué al presionar "LOGIN" no ocurre nada o da error en consola?
Asegúrese de que el archivo `md5.js` esté presente en la misma carpeta que `login.html` en el MikroTik. La función `hexMD5` contenida en `md5.js` es requerida obligatoriamente por el script de autenticación CHAP.

### 2. ¿Por qué no aparecen los íconos de usuario y contraseña?
Verifique que los archivos `usuario.svg` y `candado.svg` se encuentren en la raíz del directorio `hotspot`. La plantilla incluye estos vectores optimizados.

### 3. El navegador muestra advertencia de "Sitio no seguro" (HTTPS)
El Hotspot de MikroTik por defecto intercepta peticiones HTTP. Si los usuarios navegan a sitios HTTPS (HSTS) antes de loguearse, el navegador alertará sobre el certificado autofirmado. Para evitar esto:
- Active la detección automática de portal cautivo (Captive Portal Detection de Android / iOS / Windows).
- (Opcional) Instale un certificado SSL válido (Let's Encrypt o ZeroSSL) en `/certificate` y asócielo al Hotspot en `/ip hotspot profile set [find] ssl-certificate=su-certificado`.

### 4. No se reflejan los cambios tras subir los archivos
Los navegadores móviles suelen guardar en caché el portal cautivo. Para forzar la actualización:
- Encienda y apague el Wi-Fi en el dispositivo de prueba.
- O borre la caché del navegador para la IP del router (por ejemplo `http://192.168.88.1`).

---

## 📄 Licencia

Distribuido bajo la Licencia **MIT**. Consulte el archivo de licencia para obtener más información.
Desarrollado para la infraestructura de **WiFi Pasapalos - Caldero Sur**.
