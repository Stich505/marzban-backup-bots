# 💾 Автоматическое резервное копирование Marzban из Telegram-бот

## 🛡 Назначение
Бот создают автоматические и ручные резервные копии клиентов Marzban с отправкой уведомлений в Telegram.

## ⚙️ Что делает

- 🕛 Автоматически создаёт бэкап каждый день в 00:00 (по МСК)
- 💬 При запросе в боте вручную делает бэкап и присылает результат в бота
- 💣 При ошибке отправляет лог-файл в Telegram
- 📦 При успехе отправляет zip-архив
- ♻️ Хранит только 3 последних бэкапа
- 👤 Работает в Docker-окружении без изменений образа Marzban
- 🔐 Не шифрует архив (чтобы можно было сразу открыть)
- 📁 Архивы хранятся в `/opt/marzban_backups`

## 📦 Установка из архива

1. Скачай архив:
```bash
wget https://github.com/Stich505/marzban-backup-bot/raw/main/marzban_backup_full.tar.gz
```

2. Распакуй:
```bash
tar -xzf marzban_backup_full.tar.gz -C /
```

## 📂 Структура

```
/opt/marzban_backup_system/
└── backup.sh               # основной скрипт

/opt/marzban_backups/       # папка для хранения бэкапов

/opt/marzban_telegram_bot/
├── bot.py                  # Telegram-бот
├── config.ini              # настройки токена и пути
├── requirements.txt        # зависимости
└── venv/                   # виртуальное окружение
```

## 🛠 Настройка

1. Установи зависимости:

```bash
cd /opt/marzban_telegram_bot
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

2. Добавь Telegram-токен и ID в `config.ini`.

3. Настрой systemd-сервис:
```bash
sudo nano /etc/systemd/system/marzban-bot.service
```

4. Пример содержимого:
```ini
[Unit]
Description=Marzban Telegram Bot
After=network.target

[Service]
WorkingDirectory=/opt/marzban_telegram_bot
ExecStart=/opt/marzban_telegram_bot/venv/bin/python3 /opt/marzban_telegram_bot/bot.py
Restart=always

[Install]
WantedBy=multi-user.target
```

5. Активируй бот:
```bash
sudo systemctl daemon-reload
sudo systemctl enable marzban-bot
sudo systemctl start marzban-bot
```

## 💬 Telegram уведомления

Все сообщения (успешные и ошибки) отправляются в Telegram:

- При ошибке — лог-файл `backup.log`
- При успехе — архив бэкапа `.zip`

---

## 📄 Лицензия

Этот проект распространяется по лицензии [MIT](LICENSE). Используйте свободно, но на свой страх и риск.

(c) Stich505, 2025
