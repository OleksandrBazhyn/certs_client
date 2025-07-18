# Використовуємо Node 20 slim, бо Alpine не підтримує better-sqlite3 без додаткових танців
FROM node:20-slim

# Встановлюємо системні залежності для побудови native-модулів
RUN apt-get update && apt-get install -y python3 make g++ && apt-get clean

# Створюємо користувача для безпеки
RUN addgroup --gid 1001 nodejs && adduser --uid 1001 --ingroup nodejs --disabled-password nodejs

# Встановлюємо робочу директорію
WORKDIR /app

# Копіюємо package.json, lock та tsconfig
COPY package*.json ./
COPY tsconfig.json ./

# Встановлюємо залежності
RUN npm ci --omit=dev

# Копіюємо весь код
COPY . .

# Компілюємо TypeScript
RUN npm run build

# Створюємо директорію для логів
RUN mkdir -p /app/logs && chown -R nodejs:nodejs /app/logs

# Змінюємо користувача
USER nodejs

# Відкриваємо порт
EXPOSE 3001

# Запускаємо сервер
CMD ["node", "dist/index.js"]
