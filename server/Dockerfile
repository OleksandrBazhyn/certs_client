# Базовий образ
FROM node:20-alpine

# Робоча директорія
WORKDIR /app

# Встановлюємо залежності для native модулів
RUN apk add --no-cache python3 make g++ sqlite-dev

# Копіюємо package.json і tsconfig
COPY package*.json tsconfig.json ./

# Встановлюємо всі залежності (dev включно — бо потрібен tsc)
RUN npm ci

# Копіюємо решту коду
COPY . .

# Компілюємо TypeScript
RUN npm run build

# Створюємо користувача
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001 -G nodejs

# Дозволи для логів
RUN mkdir -p /app/logs && chown -R nodejs:nodejs /app/logs

# Користувач
USER nodejs

# Порт
EXPOSE 3001

# Запуск
CMD ["node", "dist/index.js"]
