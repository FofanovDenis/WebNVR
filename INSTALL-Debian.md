# WebNVR v1.0.5 — установка на Debian / Ubuntu

Один бинарный файл + systemd. Внешние зависимости минимальны: при установке скрипт ставит `ffmpeg` (нужен для WebRTC и H.265→H.264).

## Какой архив взять

| Архитектура | Файл | Типичное железо |
|-------------|------|-----------------|
| **amd64** | `web-nvr-linux-amd64.tar.gz` или `.zip` | обычный ПК, мини-ПК, VPS, NAS x86_64 |
| **arm64** | `web-nvr-linux-arm64.tar.gz` или `.zip` | Raspberry Pi 4/5, Orange Pi, большинство ARM64 |
| **armv7** | `web-nvr-linux-armv7.tar.gz` или `.zip` | старые 32-bit ARM (Pi 2/3 32-bit и т.п.) |

Проверить архитектуру:

```bash
dpkg --print-architecture
# или
uname -m
# x86_64 → amd64; aarch64 → arm64; armv7l → armv7
```

## Требования

- Debian 11/12/13 или Ubuntu 22.04/24.04 (и близкие)
- root / sudo
- сеть до камер (RTSP / ONVIF)
- рекомендуется от **512 МБ RAM** (удобнее от 1 ГБ)
- диск под архив (SSD предпочтительнее)

## Установка (рекомендуемый способ)

1. Скопируйте архив на сервер:

```bash
scp web-nvr-linux-amd64.tar.gz user@debian-host:/tmp/
```

2. Распакуйте и запустите установщик:

```bash
cd /tmp
tar xzf web-nvr-linux-amd64.tar.gz
cd web-nvr-linux-amd64
chmod +x install.sh update.sh
sudo ./install.sh
```

Скрипт спросит пароль администратора веб-интерфейса, поставит зависимости и `ffmpeg`, создаст пользователя `nvr`, unit systemd и каталог данных `/var/lib/web-nvr`.

Без диалога:

```bash
sudo ./install.sh --password 'ваш_надёжный_пароль'
```

Без ffmpeg (не рекомендуется — сломается WebRTC для H.265):

```bash
sudo ./install.sh --password 'пароль' --skip-ffmpeg
```

3. Откройте UI:

```text
http://IP-СЕРВЕРА:9090
```

Логин: `admin`, пароль — тот, что задали при установке.

## Что ставится

| Путь | Назначение |
|------|------------|
| `/usr/local/bin/web-nvr` | бинарник |
| `/var/lib/web-nvr/` | конфиг, БД, записи |
| `/etc/systemd/system/web-nvr.service` | автозапуск |

Сервис:

```bash
sudo systemctl status web-nvr
sudo journalctl -u web-nvr -f
```

## Обновление

Из новой папки пакета (конфиг и архив сохраняются):

```bash
sudo ./update.sh
# с зависимостями и автооткатом при сбое:
sudo ./update.sh --with-deps --auto-rollback
# откат:
sudo ./update.sh --rollback
```

## Порты

| Порт | Назначение |
|------|------------|
| **9090/tcp** | веб-интерфейс, API, WebDAV (`/dav/`) |
| **2121/tcp** | FTP (если включён) |
| **2122–2140/tcp** | FTP passive |

Если нужен доступ с других машин в LAN — откройте 9090 в firewall:

```bash
# пример ufw
sudo ufw allow 9090/tcp
```

## После установки

1. Войдите в UI → **Камеры** → **Найти устройства** (ONVIF) или **Добавить камеру** (RTSP).
2. Включите запись на нужных камерах.
3. Смотрите сетку на вкладке **Обзор**, архив — **Архив**, нагрузку — **Мониторинг**.
4. Опционально: **Настройки → Облачное соединение** для удалённого доступа через CloudWebNVR без проброса портов на роутере.

## Удаление

```bash
cd /tmp/web-nvr-linux-amd64   # каталог пакета
sudo ./install.sh --uninstall
```

Данные в `/var/lib/web-nvr` при удалении обычно сохраняются — проверьте сообщения скрипта.

## Альтернатива: сборка из исходников

Если предпочитаете собрать на самом Debian:

```bash
git clone https://github.com/SV43/WebNVR.git
cd WebNVR
chmod +x scripts/build-debian.sh
sudo ./scripts/build-debian.sh --install --password 'ваш_пароль' --with-ffmpeg
```

## Частые вопросы

**Файл `web-nvr` без расширения** — так и должно быть, это Linux-бинарник.

**Камеры H.265** — запись и HLS работают; для низколатентного WebRTC в браузере нужен `ffmpeg` (ставится по умолчанию).

**Мало места** — настройте политики хранения в **Настройки → Хранилище** / параметры камеры.

**Версия** — внизу UI: `v1.0.5`.
