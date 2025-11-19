# 1️⃣ Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

WORKDIR /src

# Copy only this project's files
COPY MyApp.csproj ./

RUN dotnet restore

# Copy all source code (now inside MyApp folder)
COPY . .

RUN dotnet publish -c Release -o /app/publish

# 2️⃣ Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0

WORKDIR /app

COPY --from=build /app/publish .

ENTRYPOINT ["dotnet", "MyApp.dll"]
