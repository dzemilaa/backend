
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-env
ARG BUILD_CONFIGURATION=Release
ENV ASPNETCORE_ENVIRONMENT=Development
WORKDIR /src

COPY ["./backend/backend.csproj", "."]
RUN dotnet restore "./backend/backend.csproj"

COPY . .
RUN dotnet build "./backend/backend.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build-env AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./backend/backend.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENV ASPNETCORE_ENVIRONMENT=Development

ENTRYPOINT ["dotnet", "./backend/backend.dll"]
