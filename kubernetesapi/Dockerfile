FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://*:8080

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine as build
WORKDIR /src
COPY ["kubernetesapi.csproj", "./"]
RUN dotnet restore "./kubernetesapi.csproj"
COPY . .
WORKDIR /src/.
RUN dotnet build "kubernetesapi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "kubernetesapi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "kubernetesapi.dll"]