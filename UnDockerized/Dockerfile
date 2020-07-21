#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["UnDockerized/UnDockerized.csproj", "UnDockerized/"]
RUN dotnet restore "UnDockerized/UnDockerized.csproj"
COPY . .
WORKDIR "/src/UnDockerized"
RUN dotnet build "UnDockerized.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "UnDockerized.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "UnDockerized.dll"]