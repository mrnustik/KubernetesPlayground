FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
ARG TARGETARCH
WORKDIR /src
COPY ["src/KubeTestApp.Api/KubeTestApp.Api.csproj", "src/KubeTestApp.Api/"]
RUN dotnet restore "src/KubeTestApp.Api/KubeTestApp.Api.csproj" -a $TARGETARCH
COPY . .
WORKDIR "/src/src/KubeTestApp.Api"
RUN dotnet build "KubeTestApp.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build -a $TARGETARCH

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
ARG TARGETARCH
RUN dotnet publish "KubeTestApp.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false -a $TARGETARCH

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "KubeTestApp.Api.dll"]
