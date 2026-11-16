# 🎮 Emuladores en Linux

Guía para instalar, configurar y optimizar emuladores, lanzadores y scripts en Linux.

> ⚠️ **Antes de continuar, lee el [Aviso Legal](#️-aviso-legal--disclaimer).**

---

## 📑 Contenido

- [Emuladores incluidos](#-emuladores-incluidos)
- [1. RPG Maker (MV, VX, VX Ace y otros)](#-1-rpg-maker-mv-vx-vx-ace-y-otros)
- [2. PCSX2 (PlayStation 2)](#️-2-pcsx2-playstation-2)
- [3. Waydroid (Android)](#-3-waydroid-android)
- [4. RetroArch (multisistema)](#-4-retroarch-multisistema)
- [Aviso Legal](#️-aviso-legal--disclaimer)

---

## 📋 Emuladores incluidos

| # | Sistema | Emulador | Estado |
|---|---|---|---|
| 1 | RPG Maker (MV, VX, VX Ace...) | [rpgmakermlinux-cicpoffs](https://github.com/bakustarver/rpgmakermlinux-cicpoffs) | ✅ Disponible |
| 2 | PlayStation 2 | [PCSX2](https://pcsx2.net/) | ✅ Disponible |
| 3 | Android | [Waydroid](https://waydro.id/) | ✅ Disponible |
| 4 | Multisistema | [RetroArch](https://www.retroarch.com/) | ✅ Disponible |

> Más emuladores se irán agregando próximamente.

---

## 🧙 1. RPG Maker (MV, VX, VX Ace y otros)

Herramienta para ejecutar juegos hechos con RPG Maker en Linux: [rpgmakermlinux-cicpoffs](https://github.com/bakustarver/rpgmakermlinux-cicpoffs).

### Instalación (Arch / Manjaro)

```bash
# 1. Instalar el lanzador
wget -qO- "https://raw.githubusercontent.com/bakustarver/rpgmakermlinux-cicpoffs/main/installgithub.sh" | bash

# 2. Instalar dependencias
sudo pacman -S libxcrypt-compat binutils
```

> 🔒 **Recomendación:** antes de ejecutar un script descargado con `| bash`, revisa su contenido. Puedes descargarlo primero con `wget` y abrirlo con un editor de texto.

### Uso

```bash
rpgmaker-linux --gamepath "$HOME/ruta/del/ejecutable/juego"
```

Cambia `$HOME/ruta/del/ejecutable/juego` por la ruta donde tengas tu juego (el que hayas obtenido de forma legal).

---

## 🕹️ 2. PCSX2 (PlayStation 2)

Emulador de PlayStation 2 de código abierto: [pcsx2.net](https://pcsx2.net/).

### Arch / Manjaro (AUR)

```bash
# Versión estable
yay -S pcsx2

# O la versión de desarrollo (última revisión de Git)
yay -S pcsx2-git
```

> `pcsx2` y `pcsx2-git` entran en conflicto entre sí: instala solo uno.

### Debian / Ubuntu y derivadas

Descarga la versión oficial desde la página del proyecto: **[pcsx2.net/downloads](https://pcsx2.net/downloads)**

Normalmente viene como AppImage:

```bash
chmod +x pcsx2-*.AppImage
./pcsx2-*.AppImage
```

### Configuración inicial

1. Abre PCSX2 y sigue el asistente de configuración.
2. **BIOS:** indica la carpeta donde guardaste tu BIOS de PS2. Debes obtenerla legalmente, extrayéndola de tu propia consola (ver aviso legal).
3. Configura tu mando y el renderizador gráfico (**Vulkan** recomendado si tu GPU lo soporta).
4. Agrega la carpeta donde tengas tus juegos, obtenidos de forma legal.

---

## 🤖 3. Waydroid (Android)

Waydroid ejecuta Android dentro de un contenedor en Linux, con acceso casi nativo al hardware. Sirve para correr apps y juegos de Android.

### Requisitos

- **Necesita Wayland.** Funciona con compositores como labwc, Sway, Hyprland, GNOME o KDE Plasma en Wayland.
- **Si usas X11, Waydroid no funciona.** Cambia a una sesión Wayland antes de seguir.
- Un kernel con soporte para `binder` (ver instalación).

### Instalación (Arch / Manjaro)

```bash
# Paquete principal (AUR)
yay -S waydroid

# Módulo del kernel binder_linux (si tu kernel no lo trae)
yay -S binder_linux-dkms
```

> En Arch a veces basta con instalar `waydroid`, ya que algunos kernels incluyen el soporte de binder. Si Waydroid no arranca, instala también `binder_linux-dkms`.

### Inicialización

```bash
# Descarga la imagen de Android y prepara el contenedor
sudo waydroid init

# Activa el servicio del contenedor
sudo systemctl enable --now waydroid-container
```

`waydroid init` **descarga la imagen de Android** (puede pesar más de 1 GB), así que necesitas conexión a internet y algo de paciencia.

### Uso

```bash
# Abrir la interfaz gráfica completa de Android
waydroid show-full-ui

# Instalar un APK
waydroid app install ruta/al/juego.apk
```

> Instala solo APKs que hayas obtenido de forma legal.

---

## 🔥 4. RetroArch (multisistema)

RetroArch es un *frontend* que reúne muchos emuladores en una sola interfaz. **Por sí solo no emula nada**: usa **cores** (núcleos), cada uno encargado de un sistema.

### Instalación (Arch / Manjaro)

```bash
sudo pacman -S retroarch
```

Los cores se pueden descargar desde la propia interfaz (ver más abajo) o instalarlos con `pacman` (los paquetes se llaman `libretro-*`).

### Configuración de vídeo y audio

Como usas Wayland, cambia los drivers para evitar tirones:

1. Ve a **Ajustes > Drivers**.
2. **Vídeo:** `wayland` (o `vulkan` si tienes una buena GPU).
3. **Audio:** `pipewire` o `pulse`.
4. Reinicia RetroArch para aplicar los cambios.

### Los cores

Para descargar cores desde el menú:

**Menú principal > Online Updater > Core Downloader**

Cores esenciales recomendados:

| Core | Sistema |
|---|---|
| **FBNeo** | Arcade (Metal Slug 3 y más) |
| **Snes9x** | Super Nintendo |
| **Genesis Plus GX** | Sega Mega Drive / Genesis |
| **mGBA** | Game Boy / GBC / GBA |

### Shaders (el toque mágico)

Para que los juegos se vean como en los arcades de verdad:

1. Con un juego abierto, entra al menú rápido y ve a **Shaders** (también en **Ajustes > Vídeo > Shaders**).
2. Carga un preset CRT, por ejemplo `crt-geom` o `zfast-crt`.
3. Guarda el preset para el juego o el core si quieres que se aplique siempre.

> Si no ves shaders, descárgalos en **Online Updater > Update Slang Shaders** (o *GLSL* según tu driver de vídeo).

### Mandos

Configura el mando **con un juego (core) abierto**:

1. Pulsa **F1** para abrir el menú rápido.
2. Ve a **Controls > Port 1 Controls**.
3. Asigna los botones.

---

## ⚠️ Aviso Legal / Disclaimer

Este repositorio tiene un propósito estrictamente educativo e informativo.

- **No busco distribuir ni alentar la piratería.** Todo el contenido aquí proporcionado se centra exclusivamente en la instalación, configuración y optimización de software (emuladores, lanzadores, scripts) para sistemas Linux.
- **No se distribuyen juegos (ROMs/ISOs) ni archivos BIOS de ningún fabricante.** Los scripts incluidos en este repositorio no contienen, ni descargan, ni enlazan a ningún tipo de material protegido por derechos de autor.
- **Responsabilidad del usuario.** Es responsabilidad exclusiva del usuario obtener los archivos BIOS y los juegos de manera legal (por ejemplo, extrayéndolos de sus propias consolas y copias físicas).
- **Marcas registradas.** Todas las marcas, logotipos y nombres de consolas mencionados pertenecen a sus respectivos dueños (Nintendo, Sony, Sega, SNK, etc.). Este proyecto no está afiliado, patrocinado ni respaldado por ninguna de estas compañías.

*El software libre es para compartir conocimiento, no para robar propiedad intelectual.*
