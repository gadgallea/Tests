FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Microservice.Core/Microservice.Core.csproj", "Microservice.Core/ "]
COPY ["Microservice.Core.Tests/Microservice.Core.Tests.csproj", "Microservice.Core.Tests/ "]
COPY ["Microservice.Infrastructure/Microservice.Infrastructure.csproj", "Microservice.Infrastructure/ "]
COPY ["Microservice.Api/Microservice.Api.csproj", "Microservice.Api/ "]

RUN dotnet restore "Microservice.Api/Microservice.Api.csproj"
COPY . .
WORKDIR "/src/Microservice.Api"
RUN dotnet build "Microservice.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Microservice.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Microservice.Api.dll"]
