# --- Stage 1: Build SDK ---
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

# نسخ ملف المشروع واستعادة الحزم
COPY *.csproj .
RUN dotnet restore

# نسخ باقي الكود وعمل Publish (بناء نسخة الإنتاج)
COPY . .
RUN dotnet publish -c Release -o /app/publish

# --- Stage 2: Final Runtime ---
FROM mcr.microsoft.com/dotnet/runtime:7.0-alpine AS final
WORKDIR /app
COPY --from=build /app/publish .

# تعديل: أضفنا السطر ده عشان ننشئ المستخدم
RUN addgroup -S app && adduser -S app -G app

# .NET images (Alpine) غالباً تنشئ مستخدم 'app' افتراضي.
USER app

ENTRYPOINT ["dotnet", "Worker.dll"]