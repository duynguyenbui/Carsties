# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:7.0 as build
WORKDIR /app
EXPOSE 80


# Copy solution file and project files
COPY Carsties.sln Carsties.sln
COPY AuctionService/AuctionService.csproj AuctionService/AuctionService.csproj
COPY SearchService/SearchService.csproj SearchService/SearchService.csproj
COPY GatewayService/GatewayService.csproj GatewayService/GatewayService.csproj
COPY Contracts/Contracts.csproj Contracts/Contracts.csproj
COPY IdentityService/IdentityService.csproj IdentityService/IdentityService.csproj
COPY BiddingService/BiddingService.csproj BiddingService/BiddingService.csproj

# Restore package dependencies
RUN dotnet restore Carsties.sln

# Copy the application source code
COPY GatewayService/ GatewayService/

# Build the application
WORKDIR /app/GatewayService
RUN dotnet publish -c Release -o /app/out

# Stage 2: Create the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /app

# Copy the published application from the build stage
COPY --from=build /app/out .

# Specify the entry point command
ENTRYPOINT [ "dotnet", "GatewayService.dll" ]
