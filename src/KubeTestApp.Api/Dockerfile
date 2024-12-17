FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG TARGETARCH
WORKDIR /src

COPY ["src/KubeTestApp.Api/KubeTestApp.Api.csproj", "src/KubeTestApp.Api/"]
RUN dotnet restore "src/KubeTestApp.Api/KubeTestApp.Api.csproj"  -a $TARGETARCH

COPY . .
WORKDIR "/src/src/KubeTestApp.Api"
RUN dotnet publish "KubeTestApp.Api.csproj" -a $TARGETARCH --no-restore -o /app


FROM mcr.microsoft.com/dotnet/aspnet:8.0
EXPOSE 8080
WORKDIR /src
COPY --link --from=build /app .
USER $APP_UID
ENTRYPOINT ["./app"]