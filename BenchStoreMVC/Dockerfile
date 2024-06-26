#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0-alpine AS base
RUN apk add --update --no-cache python3 py3-pip
RUN pip3 install --break-system-packages benchexec
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["BenchStoreMVC/BenchStoreMVC.csproj", "BenchStoreMVC/"]
COPY ["BenchStoreBL/BenchStoreBL.csproj", "BenchStoreBL/"]
COPY ["BenchStoreDAL/BenchStoreDAL.csproj", "BenchStoreDAL/"]
RUN dotnet restore "BenchStoreMVC/BenchStoreMVC.csproj"
COPY . .
WORKDIR "/src/BenchStoreMVC"
RUN dotnet build "BenchStoreMVC.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BenchStoreMVC.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BenchStoreMVC.dll"]
