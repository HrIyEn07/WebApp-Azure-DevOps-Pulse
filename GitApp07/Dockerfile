FROM mcr.microsoft.com/dotnet/aspnet:6.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 5270

ENV ASPNETCORE_URLS=http://+:5270

FROM mcr.microsoft.com/dotnet/sdk:6.0-nanoserver-1809 AS build
ARG configuration=Release
WORKDIR /src
COPY ["WebApp-Azure-DevOps-Pulse/GitApp07/GitApp07.csproj", "WebApp-Azure-DevOps-Pulse/GitApp07/"]
RUN dotnet restore "WebApp-Azure-DevOps-Pulse\GitApp07\GitApp07.csproj"
COPY . .
WORKDIR "/src/WebApp-Azure-DevOps-Pulse/GitApp07"
RUN dotnet build "GitApp07.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "GitApp07.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GitApp07.dll"]
