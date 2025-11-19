# 1️⃣ Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

WORKDIR /src

# Copy only the csproj first
COPY MyApp/MyApp.csproj ./MyApp/

RUN dotnet restore ./MyApp/MyApp.csproj

# Copy the rest of the source code
COPY MyApp ./MyApp

RUN dotnet publish ./MyApp/MyApp.csproj -c Release -o /app/publish

# 2️⃣ Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0

WORKDIR /app

COPY --from=build /app/publish .

ENTRYPOINT ["dotnet", "MyApp.dll"]
