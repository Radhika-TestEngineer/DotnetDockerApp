# syntax=docker/dockerfile:1

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy only the project files first (better layer caching)
COPY MyApp/*.csproj MyApp/
RUN dotnet restore MyApp/MyApp.csproj

# Copy the rest of the app source
COPY MyApp/. MyApp/
RUN dotnet publish MyApp/MyApp.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "MyApp.dll"]
