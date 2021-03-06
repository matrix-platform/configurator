# prepare final image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS base
WORKDIR /app
EXPOSE 80

# prepare build image
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /src

COPY Matrix.Agent.Configurator/Matrix.Agent.Configurator.csproj Matrix.Agent.Configurator/
COPY Matrix.Agent.Configurator.Business/Matrix.Agent.Configurator.Business.csproj Matrix.Agent.Configurator.Business/
COPY Matrix.Agent.Configurator.Database/Matrix.Agent.Configurator.Database.csproj Matrix.Agent.Configurator.Database/
COPY Matrix.Agent.Configurator.Model/Matrix.Agent.Configurator.Model.csproj Matrix.Agent.Configurator.Model/
COPY . .

# restore
RUN dotnet restore "Matrix.Agent.Configurator/Matrix.Agent.Configurator.csproj" -s http://nuget.matrix.paramg.com:8888/v3/index.json

# build
WORKDIR /src/Matrix.Agent.Configurator
RUN dotnet build "Matrix.Agent.Configurator.csproj" -c Release -o /app

# publish
FROM build AS publish
RUN dotnet publish "Matrix.Agent.Configurator.csproj" -c Release -o /app

# finalize
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Matrix.Agent.Configurator.dll"]