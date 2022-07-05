FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
ENV ASPNETCORE_ENVIRONMENT=Development
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Rmis.WebApi/Rmis.WebApi.csproj", "Rmis.WebApi/"]
COPY ["Rmis.Application/Rmis.Application.csproj", "Rmis.Application/"]
COPY ["Rmis.Domain/Rmis.Domain.csproj", "Rmis.Domain/"]
COPY ["Rmis.Google.Sheets/Rmis.Google.Sheets.csproj", "Rmis.Google.Sheets/"]
COPY ["Rmis.OpenWeather/Rmis.OpenWeather.csproj", "Rmis.OpenWeather/"]
COPY ["Rmis.Persistence/Rmis.Persistence.csproj", "Rmis.Persistence/"]
COPY ["Rmis.Yandex.Schedule/Rmis.Yandex.Schedule.csproj", "Rmis.Yandex.Schedule/"]
RUN dotnet restore "Rmis.WebApi/Rmis.WebApi.csproj"
COPY . .
WORKDIR "/src/Rmis.WebApi"
RUN dotnet build "Rmis.WebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Rmis.WebApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Rmis.WebApi.dll"]
