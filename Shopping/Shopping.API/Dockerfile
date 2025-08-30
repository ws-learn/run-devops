FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 5002

#listening port inside the container
ENV ASPNETCORE_URLS=http://+:5002

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Shopping/Shopping.API/Shopping.API.csproj", "Shopping/Shopping.API/"]
RUN dotnet restore "Shopping/Shopping.API/Shopping.API.csproj"
COPY . .
WORKDIR "/src/Shopping/Shopping.API"
RUN dotnet build "Shopping.API.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Shopping.API.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Shopping.API.dll"]
