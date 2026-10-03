# =========================
# Build stage
# =========================
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build

WORKDIR /src

COPY LeadManagement.API/LeadManagement.API.csproj LeadManagement.API/
COPY LeadManagement.Application/LeadManagement.Application.csproj LeadManagement.Application/
COPY LeadManagement.Domain/LeadManagement.Domain.csproj LeadManagement.Domain/
COPY LeadManagement.Infrastructure/LeadManagement.Infrastructure.csproj LeadManagement.Infrastructure/

RUN dotnet restore LeadManagement.API/LeadManagement.API.csproj

COPY LeadManagement.API/ LeadManagement.API/
COPY LeadManagement.Application/ LeadManagement.Application/
COPY LeadManagement.Domain/ LeadManagement.Domain/
COPY LeadManagement.Infrastructure/ LeadManagement.Infrastructure/

RUN dotnet publish LeadManagement.API/LeadManagement.API.csproj \
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

ENTRYPOINT ["dotnet", "LeadManagement.API.dll"]