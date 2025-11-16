# --- Stage 1: Builder ---
FROM node:18-alpine as builder

WORKDIR /app
COPY package*.json ./

# تثبيت اعتماديات الإنتاج فقط
RUN npm install --omit=dev

# --- Stage 2: Final Image ---
FROM node:18-alpine

WORKDIR /app

# إنشاء مستخدم غير جذري
RUN addgroup -S app && adduser -S app -G app

# نسخ الاعتماديات من المرحلة السابقة
COPY --from=builder /app/node_modules ./node_modules
COPY . .

# تحديد المستخدم
USER app

EXPOSE 80
CMD ["node", "server.js"]