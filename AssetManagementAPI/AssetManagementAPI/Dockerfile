# ============================================================
# Dockerfile - Kazan Neft Asset Management API
# Multi-stage build for .NET 10 on Railway
# ============================================================

# Stage 1: Build
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Copy project file and restore dependencies
COPY *.csproj ./
RUN dotnet restore

# Copy all source files and publish
COPY . ./
RUN dotnet publish -c Release -o /app/publish --no-restore

# Stage 2: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime
WORKDIR /app

# Copy published output
COPY --from=build /app/publish .

# Railway injects PORT env var
ENV ASPNETCORE_URLS=http://0.0.0.0:${PORT:-5000}

EXPOSE 5000

ENTRYPOINT ["dotnet", "KazanNeftAPI.dll"]
