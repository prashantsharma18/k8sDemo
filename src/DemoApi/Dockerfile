#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 8083

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["/src/DemoApi/DemoApi.csproj", "src/DemoApi/"]
RUN dotnet restore "src/DemoApi/DemoApi.csproj"
COPY . .
WORKDIR "/src/src/DemoApi"
RUN dotnet build "DemoApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DemoApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS http://*:8083
ENTRYPOINT ["dotnet", "DemoApi.dll"]
#ENTRYPOINT ["dotnet", "run", "--server.urls", "http://*:8083"]