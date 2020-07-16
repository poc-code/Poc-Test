#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["ElasticSearch/src/PocTestes.ElasticSearch.Api/PocTestes.ElasticSearch.Api.csproj", "ElasticSearch/src/PocTestes.ElasticSearch.Api/"]
RUN dotnet restore "ElasticSearch/src/PocTestes.ElasticSearch.Api/PocTestes.ElasticSearch.Api.csproj"
COPY . .
WORKDIR "/src/ElasticSearch/src/PocTestes.ElasticSearch.Api"
RUN dotnet build "PocTestes.ElasticSearch.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PocTestes.ElasticSearch.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PocTestes.ElasticSearch.Api.dll"]