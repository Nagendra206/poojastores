# Use the official .NET SDK as the base image
FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build

# Set the working directory in the container
WORKDIR /app

# Copy the .NET project files to the container
COPY *.csproj ./

# Restore the project dependencies
RUN dotnet restore

# Copy the entire application code to the container
COPY . ./

# Build the application
RUN dotnet build -c Release --no-restore

# Publish the application to a folder
RUN dotnet publish -c Release -o out --no-restore

# Create a runtime image
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS runtime

# Set the working directory in the container
WORKDIR /app

# Copy the published files from the build stage
COPY --from=build /app/out ./

# Set the entry point for the application
ENTRYPOINT ["dotnet", "PoojaStores.dll"]
