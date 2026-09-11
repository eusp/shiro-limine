# shiro-limine

Tema del bootloader Limine del escritorio Shiro (el que CachyOS instala por defecto). Forma parte de
[shiro-theme](https://github.com/eusp/shiro-theme): `background.png` y `theme.conf` los genera
`builders/limine.js` con los colores del tema activo, así que no conviene editarlos a mano.

Proyectos Shiro: [shiro-theme](https://github.com/eusp/shiro-theme) ·
[shiro-ags](https://github.com/eusp/shiro-ags) ·
[shiro-hyprland](https://github.com/eusp/shiro-hyprland) ·
[shiro-sddm](https://github.com/eusp/shiro-sddm) ·
[shiro-grub](https://github.com/eusp/shiro-grub) ·
**shiro-limine**

## Instalación

El repo vive en `~/.config/shiro-limine` (lo clona `install.sh` de shiro-theme). Para aplicarlo:

```bash
sudo node ~/.config/shiro-theme/build-boot.js
```

`build-boot.js` detecta el bootloader: con Limine, el builder

1. busca `limine.conf` en el ESP (`/boot`, `/efi`, `/boot/efi`; o la ruta de la variable `LIMINE_CONF`),
2. copia el fondo a `<carpeta de limine.conf>/shiro-limine/background.png`,
3. inserta `theme.conf` al principio de `limine.conf`, entre las marcas `# >>> shiro-limine` y
   `# <<< shiro-limine`, y comenta con `# (shiro-limine)` las opciones de colores de la distro que
   pisaría. Las entradas de arranque no se tocan.

Es idempotente: al cambiar de tema se reemplaza el bloque. Respaldos junto a `limine.conf`:
`limine.conf.bak` (antes de la última aplicación) y `limine.conf.shiro-orig` (el original de la distro).
Para quitar el tema: `sudo cp limine.conf.shiro-orig limine.conf`.

## Qué se puede personalizar

Limine es más simple que GRUB: no permite ubicar el menú, ni imágenes de selección, ni iconos, ni
fuentes TTF. El tema cambia el fondo, el texto de marca, los colores de ayuda y la paleta del menú
(`term_palette`, `term_foreground`, etc.), con el fondo del menú transparente sobre el wallpaper.

## Secure Boot

Si `/etc/default/limine` tiene `ENABLE_ENROLL_LIMINE_CONFIG=yes`, el hash de `limine.conf` está
grabado en el binario de Limine, y editarlo sin volver a enrolarlo deja el sistema sin arrancar. En
ese caso el builder **no toca nada** y avisa.
