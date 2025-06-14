# Настройка MongoDB для Rust Server Monitor

## Вариант 1: Локальная установка MongoDB (Рекомендуется для разработки)

### Windows

1. **Скачайте MongoDB Community Server:**
   - Перейдите на https://www.mongodb.com/try/download/community
   - Выберите версию для Windows
   - Скачайте и установите .msi файл

2. **Установка:**
   - Запустите установщик MongoDB
   - Выберите "Complete" installation
   - Установите MongoDB как Windows Service
   - Установите MongoDB Compass (GUI для управления БД)

3. **Запуск MongoDB:**
   ```powershell
   # MongoDB должен запуститься автоматически как сервис
   # Проверить статус:
   Get-Service -Name MongoDB
   
   # Если не запущен, запустить:
   Start-Service -Name MongoDB
   ```

4. **Проверка подключения:**
   ```powershell
   # Откройте MongoDB Compass и подключитесь к:
   # mongodb://localhost:27017
   ```

## Вариант 2: MongoDB Atlas (Облачная БД)

1. **Создайте аккаунт на MongoDB Atlas:**
   - Перейдите на https://www.mongodb.com/atlas
   - Зарегистрируйтесь

2. **Создайте кластер:**
   - Выберите Free tier (M0)
   - Выберите регион (ближайший к вам)
   - Создайте кластер

3. **Настройте доступ:**
   - Database Access → Add New Database User
   - Network Access → Add IP Address (0.0.0.0/0 для разработки)

4. **Получите строку подключения:**
   - Connect → Connect your application
   - Скопируйте connection string

5. **Обновите backend/config/config.env:**
   ```env
   MONGODB_URL=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/rust-monitoring
   ```

## Вариант 3: Docker (Быстрый запуск)

1. **Установите Docker Desktop** с официального сайта

2. **Запустите MongoDB в контейнере:**
   ```bash
   docker run -d --name mongodb -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password mongo:latest
   ```

3. **Обновите config.env:**
   ```env
   MONGODB_URL=mongodb://admin:password@localhost:27017/rust-monitoring?authSource=admin
   ```

## Проверка установки

После настройки MongoDB запустите проект:

```bash
# В папке backend
npm start
```

Если вы видите сообщение "✅ Подключение к MongoDB установлено" - все настроено правильно!

## Первый запуск

1. Запустите backend сервер
2. Подождите 5-10 минут для первого парсинга серверов из Facepunch API
3. Откройте MongoDB Compass и проверьте базу данных `rust-monitoring`
4. Вы должны увидеть коллекции `servers` и `users` с данными

## Устранение проблем

### Ошибка подключения к MongoDB
- Убедитесь, что MongoDB запущен
- Проверьте строку подключения в config.env
- Для Atlas: проверьте Network Access и Database Access

### Нет серверов в базе
- Проверьте логи backend сервера
- Убедитесь, что STEAM_API_KEY настроен
- Проверьте подключение к интернету

### Низкая производительность
- Для локальной MongoDB создайте индексы:
  ```javascript
  // В MongoDB Compass или mongosh
  db.servers.createIndex({ "ip": 1, "port": 1 })
  db.servers.createIndex({ "isOnline": 1, "isVisible": 1 })
  db.servers.createIndex({ "stats.rank": 1 })
  ```

## Рекомендации для продакшена

1. Используйте MongoDB Atlas или выделенный сервер
2. Настройте регулярные бэкапы
3. Создайте все необходимые индексы
4. Мониторьте производительность базы данных
5. Настройте репликацию для высокой доступности 