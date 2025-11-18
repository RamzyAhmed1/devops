# --- Stage 1: Build SDK ---
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

# Copy the project file and restore dependencies
COPY *.csproj .
RUN dotnet restore

# Copy the remaining source code and publish the production build
COPY . .
RUN dotnet publish -c Release -o /app/publish

# --- Stage 2: Final Runtime ---
FROM mcr.microsoft.com/dotnet/runtime:7.0-alpine AS final
WORKDIR /app
COPY --from=build /app/publish .

# Added this step to create a dedicated non-root user
RUN addgroup -S app && adduser -S app -G app

# Alpine .NET runtime images typically include an 'app' user already
USER app

ENTRYPOINT ["dotnet", "Worker.dll"]
