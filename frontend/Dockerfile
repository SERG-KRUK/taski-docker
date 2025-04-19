# Используем официальный образ Node.js на базе Alpine (легковесный)
FROM node:16-alpine as builder

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем только файлы, необходимые для установки зависимостей
COPY package.json package-lock.json ./

# Устанавливаем зависимости
RUN npm ci

# Копируем все остальные файлы проекта
COPY . .

# Собираем проект
RUN npm run build

# Финальный образ для запуска
FROM node:16-alpine

WORKDIR /app

# Копируем только собранные файлы и необходимые для запуска
COPY --from=builder /app/build ./build
COPY --from=builder /app/node_modules ./node_modules

# Устанавливаем простой HTTP-сервер для раздачи статики
RUN npm install -g serve

# Открываем порт 8000
EXPOSE 8000

# Запускаем сервер для раздачи статики
CMD ["serve", "-s", "build", "-l", "8000"]