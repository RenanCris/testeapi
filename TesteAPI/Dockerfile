# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/dotnet/sdk:5.0 as build
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore
COPY . .
WORKDIR /app/.
RUN dotnet build -c Release -o /app/out-build

# Copy everything else and build
FROM  build as publish
RUN dotnet publish -c Release -o /app/out-publish

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:5.0 as final
WORKDIR /app
COPY --from=publish /app/out-publish .
ENTRYPOINT ["dotnet", "TesteAPI.dll"]