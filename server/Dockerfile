FROM node:20-slim

# Встановлюємо системні залежності для побудови native-модулів
RUN apt-get update && apt-get install -y python3 make g++ && apt-get clean

# Створюємо користувача
RUN addgroup --gid 1001 nodejs && adduser --uid 1001 --ingroup nodejs --disabled-password nodejs

WORKDIR /app

COPY package*.json ./
COPY tsconfig.json ./

# Встановлюємо всі залежності (включно з dev)
RUN npm ci

# Компілюємо TypeScript
COPY . .
RUN npm run build

# Видаляємо dev-залежності після білду
RUN npm prune --omit=dev

# Права доступу + лог-директорія
RUN mkdir -p /app/logs && chown -R nodejs:nodejs /app/logs

USER nodejs
EXPOSE 3001

CMD ["node", "dist/index.js"]
