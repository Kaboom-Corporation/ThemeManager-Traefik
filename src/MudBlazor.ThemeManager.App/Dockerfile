#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/MudBlazor.ThemeManager.App/MudBlazor.ThemeManager.App.csproj", "MudBlazor.ThemeManager.App/"]
COPY ["src/MudBlazor.ThemeManager/MudBlazor.ThemeManager.csproj", "MudBlazor.ThemeManager/"]
COPY ["src/MudBlazor.ThemeManager.sln", ""]
RUN dotnet restore "MudBlazor.ThemeManager.App/MudBlazor.ThemeManager.App.csproj"
COPY . .
WORKDIR "/src/MudBlazor.ThemeManager.App"
RUN dotnet build "MudBlazor.ThemeManager.App.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MudBlazor.ThemeManager.App.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MudBlazor.ThemeManager.App.dll"]