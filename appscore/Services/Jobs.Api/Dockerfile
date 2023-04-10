FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
COPY Services/Jobs.Api/jobs.api.csproj /src/Services/Jobs.Api/
COPY Foundation/Events/Events.csproj /src/Foundation/Events/

RUN dotnet restore /src/Services/Jobs.Api/jobs.api.csproj 
COPY Services/Jobs.Api /src/Services/Jobs.Api/
COPY Foundation/Events /src/Foundation/Events/
RUN cd /src/Services/Jobs.Api && dotnet build jobs.api.csproj -c Release -o /app/build

FROM build AS publish
RUN dotnet publish /src/Services/Jobs.Api/jobs.api.csproj -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "jobs.api.dll"]