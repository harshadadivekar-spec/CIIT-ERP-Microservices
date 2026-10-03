# =========================
# Build stage
# =========================
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build

WORKDIR /src

# Copy project files first
COPY DeveloperManagement.API/DeveloperManagement.API.csproj DeveloperManagement.API/
COPY DeveloperManagement.Application/DeveloperManagement.Application.csproj DeveloperManagement.Application/
COPY DeveloperManagement.Domain/DeveloperManagement.Domain.csproj DeveloperManagement.Domain/
COPY DeveloperManagement.Infrastructure/DeveloperManagement.Infrastructure.csproj DeveloperManagement.Infrastructure/

# Restore dependencies
RUN dotnet restore DeveloperManagement.API/DeveloperManagement.API.csproj

# Copy source code
COPY DeveloperManagement.API/ DeveloperManagement.API/
COPY DeveloperManagement.Application/ DeveloperManagement.Application/
COPY DeveloperManagement.Domain/ DeveloperManagement.Domain/
COPY DeveloperManagement.Infrastructure/ DeveloperManagement.Infrastructure/

# Publish
RUN dotnet publish DeveloperManagement.API/DeveloperManagement.API.csproj \
    -c Release \
    -o /app/publish \
    --no-restore

# =========================
# Runtime stage
# =========================
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final

WORKDIR /app

COPY --from=build /app/publish .

ENV ASPNETCORE_URLS=http://+:8080

EXPOSE 8080

ENTRYPOINT ["dotnet", "DeveloperManagement.API.dll"]