#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["it_tizes/valami/valami.csproj", "it_tizes/valami/"]
RUN dotnet restore "it_tizes/valami/valami.csproj"
COPY . .
WORKDIR "/src/it_tizes/valami"
RUN dotnet build "valami.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "valami.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "valami.dll"]