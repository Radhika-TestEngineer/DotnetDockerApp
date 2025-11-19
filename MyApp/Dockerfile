# 1️⃣ Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

WORKDIR /src

# Copy only the csproj first
COPY MyApp.csproj .

RUN dotnet restore

# Copy rest of the code
COPY . .

RUN dotnet publish -c Release -o /app

# 2️⃣ Runtime stage (IMPORTANT FIX!)
FROM mcr.microsoft.com/dotnet/aspnet:8.0

WORKDIR /app

COPY --from=build /app ./

ENTRYPOINT ["dotnet", "MyApp.dll"]
