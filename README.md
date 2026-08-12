## WebNVR
NVR в одном бинарнике для Debian/Ubuntu, Raspberry Pi и мини-ПК. Веб-интерфейс, локальный архив, CloudWebNVR без проброса портов.

**Лицензия:** proprietary / all rights reserved (см. `LICENSE.txt` в релизе).

### Что нового

- **Брендинг** — логотипы WebNVR / CloudWebNVR, favicon, PWA
- **Превью сетки** — приоритет HLS для стабильной многокамерной сетки; WebCodecs как запасной вариант
- **SQLite** — убран `mmap_size` при открытии (защита от SEGV), авто-восстановление stale WAL/`-shm` после крэша
- **UI** — focus-visible, правки плеера и архива
- **Лицензия** — проект переведён на проприетарные условия (MIT больше не действует на WebNVR целиком)

### Скачать

| Архитектура | Архив | Типичное железо |
|-------------|--------|-----------------|
| **amd64** | `web-nvr-linux-amd64.tar.gz` / `.zip` | ПК, мини-ПК, VPS, NAS x86_64 |
| **arm64** | `web-nvr-linux-arm64.tar.gz` / `.zip` | Raspberry Pi 4/5, ARM64 |
| **armv7** | `web-nvr-linux-armv7.tar.gz` / `.zip` | старые 32-bit ARM |

Также в релизе: `INSTALL-Debian.md`, `LICENSE.txt`, `SHA256SUMS.txt`, `webnvr-1.0.5-4pda-docs.zip`.

Проверка архитектуры: `dpkg --print-architecture`

### Установка (Debian / Ubuntu)

```bash
cd /tmp
tar xzf web-nvr-linux-amd64.tar.gz
cd web-nvr-linux-amd64
chmod +x install.sh update.sh
sudo ./install.sh
# или: sudo ./install.sh --password 'your_password'
```

Открыть: `http://IP:9090` — логин `admin`, пароль из установки.

Обновление (конфиг и записи сохраняются):

```bash
sudo ./update.sh
```

Подробно — `INSTALL-Debian.md` в комплекте.

### Важно

- После установки смените пароль; не выставляйте `:9090` в интернет без защиты (CloudWebNVR / VPN / reverse proxy + HTTPS).
- Для H.265 + WebRTC нужен `ffmpeg` — установщик ставит его по умолчанию.
- Контрольные суммы: `SHA256SUMS.txt`.

### Feedback

Укажите модель камер, ОС, архитектуру и `journalctl -u web-nvr -n 100`.
