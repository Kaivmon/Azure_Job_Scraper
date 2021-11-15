#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Azure_Job_Scraper/Azure_Job_Scraper.csproj", "Azure_Job_Scraper/"]
RUN dotnet restore "Azure_Job_Scraper/Azure_Job_Scraper.csproj"
COPY . .
WORKDIR "/src/Azure_Job_Scraper"
RUN dotnet build "Azure_Job_Scraper.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Azure_Job_Scraper.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Azure_Job_Scraper.dll"]