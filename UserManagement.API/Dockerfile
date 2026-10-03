# =========================
# Build stage
# =========================
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build

WORKDIR /src

COPY UserManagement.API/UserManagement.API.csproj UserManagement.API/
COPY UserManagement.Application/UserManagement.Application.csproj UserManagement.Application/
COPY UserManagement.Domain/UserManagement.Domain.csproj UserManagement.Domain/
COPY UserManagement.Infrastructure/UserManagement.Infrastructure.csproj UserManagement.Infrastructure/

RUN dotnet restore UserManagement.API/UserManagement.API.csproj

COPY UserManagement.API/ UserManagement.API/
COPY UserManagement.Application/ UserManagement.Application/
COPY UserManagement.Domain/ UserManagement.Domain/
COPY UserManagement.Infrastructure/ UserManagement.Infrastructure/

RUN dotnet publish UserManagement.API/UserManagement.API.csproj \
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

ENTRYPOINT ["dotnet", "UserManagement.API.dll"]