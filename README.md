## WebNVR v1.1.2

Профессиональный NVR в одном бинарнике для Debian/Ubuntu, Raspberry Pi и мини-ПК. Веб-интерфейс, локальный архив, CloudWebNVR без проброса портов.

**Лицензия:** proprietary / all rights reserved (см. `LICENSE`).

### Скриншоты

| Обзор | Архив |
|:-----:|:-----:|
| ![Сетка камер](https://github.com/FofanovDenis/WebNVR/releases/download/v1.1.2/nvr-surveillance.png) | ![Архив](https://github.com/FofanovDenis/WebNVR/releases/download/v1.1.2/nvr-archive.png) |
| **Камеры** | **Мониторинг** |
| ![Список камер](https://github.com/FofanovDenis/WebNVR/releases/download/v1.1.2/nvr-cameras.png) | ![Дашборд](https://github.com/FofanovDenis/WebNVR/releases/download/v1.1.2/nvr-dashboard.png) |

### Что нового

#### Архив как видеорегистратор
- Страница записей — рабочее место DVR: плеер + шкала покрытия дня с переходом между сегментами
- Управление воспроизведением и нарезкой: скорость, метки клипа, часы поверх кадра
- Облачный архив совпадает с WebNVR: сцена 16:9, оверлей загрузки/ошибки, play по клику
- Скачивание архива с `Content-Type: video/mp4`

#### Интерфейс
- Обновлены шапка, Dashboard, карточки камер, библиотека, пагинация, Настройки и сетка наблюдения
- Хром плеера: одинаковая высота кнопок (34px), ровный промежуток, `1x` по центру, mute не прижат к краю
- Мониторинг NVR и облака — та же ширина контента, что у Настроек
- CloudWebNVR визуально совпадает с оболочкой NVR

#### Сетка камер (локально и в облаке)
- Ячейки строго **16:9**, по центру экрана; видео больше не схлопывает плитки
- Онлайн-камеры показывают **live** (запись на диск — не отдельный статус в сетке)
- Тот же основной HLS, что и в Live View; HEVC при ошибке откатывается на preview

#### Live / HLS / статистика
- Локальный HLS переключает preview (sub) ↔ live (main) без залипания muxer
- `GET /api/stats/system` отдаёт CPU % и скорость сети; коллекторы для Windows

#### CloudWebNVR
- Бейдж записи учитывает `recording_enabled` на агенте
- Скриншоты сайта обновлены (NVR + облако)

Полный список: [CHANGELOG.md](https://github.com/SV43/WebNVR/blob/main/CHANGELOG.md)

### Скачать

**WebNVR (регистратор)**

| Архитектура | Архив | Типичное железо |
|-------------|--------|-----------------|
| **amd64** | `web-nvr-linux-amd64.tar.gz` / `.zip` | ПК, мини-ПК, VPS, NAS x86_64 |
| **arm64** | `web-nvr-linux-arm64.tar.gz` / `.zip` | Raspberry Pi 4/5, ARM64 |
| **armv7** | `web-nvr-linux-armv7.tar.gz` / `.zip` | старые 32-bit ARM |

**CloudWebNVR (релей)**

| Архитектура | Архив |
|-------------|--------|
| **amd64** | `webnvr-cloud-relay-linux-amd64.tar.gz` / `.zip` |
| **arm64** | `webnvr-cloud-relay-linux-arm64.tar.gz` / `.zip` |
| **armv7** | `webnvr-cloud-relay-linux-armv7.tar.gz` / `.zip` |

Проверка архитектуры: `dpkg --print-architecture`

### Установка WebNVR (Debian / Ubuntu)

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

### Установка CloudWebNVR (Ubuntu VPS)

```bash
cd /tmp
unzip webnvr-cloud-relay-linux-amd64.zip
cd webnvr-cloud-relay-linux-amd64
chmod +x install.sh cloud-relay
sudo ./install.sh --domain cloud.example.com --password 'UiSecret'
```

После установки: `https://cloud.example.com/`

### Важно

- После установки смените пароль; не выставляйте `:9090` в интернет без защиты (CloudWebNVR / VPN / reverse proxy + HTTPS).
- Для H.265 + WebRTC нужен `ffmpeg` — установщик ставит его по умолчанию.
- Официальный агент подключается к `ws://cloud.idist.ru:9100/v1/agent/ws`. Свой релей без `-vendor-key` с release-агентом не стыкуется.

### Обратная связь

Issues: https://github.com/SV43/WebNVR/issues  
Укажите модель камер, ОС, архитектуру и `journalctl -u web-nvr -n 100`.
