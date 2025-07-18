# 🛠️ Стадія збірки
FROM node:18-alpine AS builder

# Встановлюємо робочу директорію
WORKDIR /app

# Копіюємо лише package.json та lock-файл
COPY package*.json ./

# Встановлюємо залежності
RUN npm ci

# Копіюємо решту файлів (після встановлення залежностей для кешу)
COPY . .

# Збираємо додаток
RUN npm run build

# Стадія продакшну
FROM nginx:alpine

# Видаляємо стандартну конфігурацію nginx (опційно, залежить від твоєї nginx.conf)
RUN rm -rf /etc/nginx/conf.d/default.conf

# Копіюємо власну конфігурацію nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Копіюємо збудовану апку
COPY --from=builder /app/build /usr/share/nginx/html

# Відкриваємо порт
EXPOSE 80

# Стартуємо nginx
CMD ["nginx", "-g", "daemon off;"]
