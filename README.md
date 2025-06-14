# 🦀 Rust Server Monitor

Современный мониторинг серверов Rust с аутентификацией через Steam, рейтинговой системой и полным функционалом управления серверами.

## ✨ Основные возможности

### 🔐 Аутентификация и профили
- **Авторизация через Steam** - безопасный вход через Steam OpenID
- **Управление профилем** - настройка аккаунта и приватности
- **Система ролей** - пользователи, модераторы, администраторы

### 🖥️ Мониторинг серверов
- **Реальный онлайн** - актуальная информация о количестве игроков
- **История вайпов** - отслеживание последних и следующих вайпов
- **Геолокация** - определение страны сервера по IP
- **Теги серверов** - vanilla, modded, PvE, PvP, solo/duo/trio и др.
- **Статистика активности** - uptime, пиковые значения игроков

### 📊 Рейтинговая система
- **Автоматические ранги** - расчет на основе активности и популярности
- **Голосование игроков** - возможность голосовать за любимые серверы
- **Топы по категориям** - популярные, новые, рекомендуемые серверы

### 🎛️ Управление серверами
- **Добавление серверов** - простая регистрация с автоматическим определением данных
- **Верификация владения** - через изменение названия или RCON доступ
- **Настройка информации** - описание, теги, социальные сети, медиа
- **Расписание вайпов** - автоматическое отслеживание циклов

### 🎨 Современный дизайн
- **Стиль игры Rust** - темная тема с оранжевыми акцентами
- **Адаптивность** - корректная работа на всех устройствах
- **Анимации** - плавные переходы и интерактивность
- **Быстродействие** - оптимизированная загрузка и кэширование

## 🛠️ Технологический стек

### Backend
- **Node.js** + **Express.js** - серверная часть
- **MongoDB** + **Mongoose** - база данных
- **Passport.js** - аутентификация через Steam
- **GameDig** - опрос игровых серверов
- **Node-cron** - планировщик задач
- **Winston** - система логирования
- **RCON** - управление серверами

### Frontend
- **React 18** - пользовательский интерфейс
- **Vite** - сборщик и dev-сервер
- **TailwindCSS** - стилизация
- **Framer Motion** - анимации
- **React Query** - управление состоянием API
- **React Router** - маршрутизация
- **React Hook Form** - формы

## 🚀 Быстрый старт

### Предварительные требования

- **Node.js** 18+ 
- **MongoDB** 6+
- **Steam API Key** (получить на [steamcommunity.com](https://steamcommunity.com/dev/apikey))

### 1. Клонирование репозитория

```bash
git clone https://github.com/yourusername/rust-server-monitor.git
cd rust-server-monitor
```

### 2. Установка зависимостей

```bash
# Установка зависимостей для всего проекта
npm install

# Установка зависимостей для backend
cd backend
npm install

# Установка зависимостей для frontend
cd ../frontend
npm install
cd ..
```

### 3. Настройка переменных окружения

Скопируйте файл конфигурации и настройте его:

```bash
cp backend/config.env backend/.env
```

Отредактируйте `backend/.env`:

```env
# База данных
MONGODB_URI=mongodb://localhost:27017/rust-server-monitor

# Steam API
STEAM_API_KEY=ваш_steam_api_ключ
STEAM_RETURN_URL=http://localhost:3000/auth/success

# Безопасность
JWT_SECRET=очень_секретный_ключ_для_jwt
SESSION_SECRET=очень_секретный_ключ_для_сессий

# Сервер
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:3000
```

### 4. Запуск MongoDB

Убедитесь, что MongoDB запущена:

```bash
# Ubuntu/Debian
sudo systemctl start mongod

# Windows (если установлена как сервис)
net start MongoDB

# Или запустите напрямую
mongod
```

### 5. Запуск приложения

Запустите бэкенд и фронтенд одновременно:

```bash
npm run dev
```

Или запустите по отдельности:

```bash
# Терминал 1 - Backend
cd backend
npm run dev

# Терминал 2 - Frontend
cd frontend
npm run dev
```

### 6. Доступ к приложению

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **Health Check**: http://localhost:5000/api/health

## 📁 Структура проекта

```
rust-server-monitor/
├── backend/                 # Серверная часть
│   ├── src/
│   │   ├── config/         # Конфигурация (DB, Passport)
│   │   ├── models/         # Модели данных (User, Server)
│   │   ├── routes/         # API маршруты
│   │   ├── services/       # Бизнес-логика
│   │   ├── utils/          # Утилиты (логгер)
│   │   └── server.js       # Точка входа
│   ├── package.json
│   └── config.env          # Конфигурация
├── frontend/               # Клиентская часть
│   ├── src/
│   │   ├── components/     # React компоненты
│   │   ├── pages/          # Страницы приложения
│   │   ├── hooks/          # Пользовательские хуки
│   │   ├── api/            # API клиент
│   │   ├── utils/          # Утилиты
│   │   ├── App.jsx         # Главный компонент
│   │   └── main.jsx        # Точка входа
│   ├── index.html
│   ├── package.json
│   ├── tailwind.config.js  # Конфигурация TailwindCSS
│   └── vite.config.js      # Конфигурация Vite
├── package.json            # Корневой package.json
└── README.md              # Документация
```

## 🔧 Основные API эндпоинты

### Аутентификация
- `GET /api/auth/steam` - Вход через Steam
- `GET /api/auth/me` - Текущий пользователь
- `POST /api/auth/logout` - Выход

### Серверы
- `GET /api/servers` - Список серверов
- `POST /api/servers` - Добавить сервер
- `GET /api/servers/:id` - Детали сервера
- `POST /api/servers/:id/vote` - Голосовать за сервер
- `POST /api/servers/:id/verify` - Верифицировать сервер

### Пользователи
- `GET /api/users/:id` - Профиль пользователя
- `GET /api/users/leaderboard/server-owners` - Топ владельцев

### Администрирование
- `GET /api/admin/stats` - Статистика системы
- `GET /api/admin/users` - Управление пользователями
- `GET /api/admin/servers` - Управление серверами

## 🚦 Переменные окружения

### Backend (.env)
| Переменная | Описание | Обязательна |
|------------|----------|-------------|
| `MONGODB_URI` | URL подключения к MongoDB | ✅ |
| `STEAM_API_KEY` | Ключ Steam API | ✅ |
| `JWT_SECRET` | Секретный ключ для JWT | ✅ |
| `SESSION_SECRET` | Секретный ключ для сессий | ✅ |
| `PORT` | Порт сервера | ❌ (5000) |
| `NODE_ENV` | Окружение | ❌ (development) |
| `FRONTEND_URL` | URL фронтенда | ❌ |

## 🧪 Тестирование

```bash
# Тестирование backend
cd backend
npm test

# Тестирование frontend
cd frontend
npm test

# Проверка типов
npm run type-check

# Линтинг
npm run lint
```

## 📦 Сборка для продакшена

```bash
# Сборка всего проекта
npm run build

# Только frontend
cd frontend
npm run build

# Только backend
cd backend
npm run build
```

## 🐳 Docker (Опционально)

```bash
# Сборка образов
docker-compose build

# Запуск в контейнерах
docker-compose up -d

# Остановка
docker-compose down
```

## 🤝 Contributing

1. Форкните репозиторий
2. Создайте ветку для фичи (`git checkout -b feature/amazing-feature`)
3. Зафиксируйте изменения (`git commit -m 'Add amazing feature'`)
4. Запушьте ветку (`git push origin feature/amazing-feature`)
5. Откройте Pull Request

## 📝 Лицензия

Этот проект распространяется под лицензией MIT. См. файл `LICENSE` для подробностей.

## 🆘 Поддержка

Если у вас есть вопросы или проблемы:

1. Проверьте [Issues](https://github.com/yourusername/rust-server-monitor/issues)
2. Создайте новый Issue с подробным описанием
3. Присоединяйтесь к нашему [Discord серверу](https://discord.gg/your-server)

## 🔮 Планы развития

- [ ] **Мобильное приложение** (React Native)
- [ ] **WebSocket** для реального времени
- [ ] **Карты серверов** с живой информацией
- [ ] **Система уведомлений** о вайпах
- [ ] **API для разработчиков**
- [ ] **Интеграция с Discord ботами**
- [ ] **Система рецензий** на серверы
- [ ] **Расширенная аналитика**

---

Сделано с ❤️ для сообщества Rust 