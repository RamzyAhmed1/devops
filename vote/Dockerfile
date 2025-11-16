# --- Stage 1: Builder ---
# تعديل: استخدمنا الصورة الكاملة (شيلنا -slim)
FROM python:3.10-bullseye as builder

WORKDIR /app

# إنشاء مستخدم غير جذري (non-root)
RUN addgroup --system app && adduser --system --group app

# نسخ ملف الاعتماديات أولاً للاستفادة من الكاش
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# --- Stage 2: Final Image ---
# تعديل: استخدمنا الصورة الكاملة (شيلنا -slim)
FROM python:3.10-bullseye

WORKDIR /app

# نسخ المستخدم والاعتماديات من المرحلة السابقة
COPY --from=builder /etc/passwd /etc/passwd
COPY --from=builder /etc/group /etc/group
COPY --from=builder /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# نسخ كود التطبيق
COPY . .

# تحديد المستخدم غير الجذري
USER app

# تشغيل التطبيق
CMD ["flask", "run", "--host=0.0.0.0", "--port=80"]