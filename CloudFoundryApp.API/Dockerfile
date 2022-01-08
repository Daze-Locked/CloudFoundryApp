#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["CloudFoundryApp.API/CloudFoundryApp.API.csproj", "CloudFoundryApp.API/"]
RUN dotnet restore "CloudFoundryApp.API/CloudFoundryApp.API.csproj"
COPY . .
WORKDIR "/src/CloudFoundryApp.API"
RUN dotnet build "CloudFoundryApp.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CloudFoundryApp.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CloudFoundryApp.API.dll"]
