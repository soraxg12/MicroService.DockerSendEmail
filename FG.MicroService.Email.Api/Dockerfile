#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["FG.MicroService.Email.Api/FG.MicroService.Email.Api.csproj", "FG.MicroService.Email.Api/"]
RUN dotnet restore "./FG.MicroService.Email.Api/./FG.MicroService.Email.Api.csproj"
COPY . .
WORKDIR "/src/FG.MicroService.Email.Api"
RUN dotnet build "./FG.MicroService.Email.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./FG.MicroService.Email.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FG.MicroService.Email.Api.dll"]