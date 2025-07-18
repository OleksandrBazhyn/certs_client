# Базовий образ
FROM node:20-alpine

# Робоча директорія
WORKDIR /app

# Встановлюємо залежності для native модулів
RUN apk add --no-cache python3 make g++ sqlite-dev

# Копіюємо всі файли
COPY . .

# Встановлюємо всі залежності та будуємо проект
RUN npm ci && npm run build

# Створюємо користувача
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001 -G nodejs

# Змінюємо власника файлів
RUN chown -R nodejs:nodejs /app

# Перемикаємося на непривілейованого користувача
USER nodejs

# Дозволи для логів та бази даних
RUN mkdir -p /app/logs /app/data && chown -R nodejs:nodejs /app/logs /app/data

# Користувач
USER nodejs

# Порт
EXPOSE 3001

# Запуск
CMD ["node", "dist/index.js"]
