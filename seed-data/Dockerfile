# هنستخدم صورة بايثون خفيفة
FROM python:3.10-alpine

WORKDIR /app

# هنثبت الأدوات المطلوبة
RUN apk add --no-cache apache2-utils dos2unix

# ننسخ كل السكربتات
COPY . .

# نصلح الملفات (احتياطي) ونخليهم قابلين للتشغيل
RUN dos2unix make-data.py \
    && dos2unix generate-votes.sh \
    && chmod +x generate-votes.sh

# تعديل: هنشغل بايثون الأول، وبعدين السكربت
CMD ["sh", "-c", "python make-data.py && ./generate-votes.sh"]  