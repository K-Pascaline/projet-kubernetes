FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
COPY Services/Applicants.Api/applicants.api.csproj /src/Services/Applicants.Api/
COPY Foundation/Events/Events.csproj /src/Foundation/Events/

RUN dotnet restore /src/Services/Applicants.Api/applicants.api.csproj 
COPY Services/Applicants.Api /src/Services/Applicants.Api/
COPY Foundation/Events /src/Foundation/Events/
RUN cd /src/Services/Applicants.Api && dotnet build applicants.api.csproj -c Release -o /app/build

FROM build AS publish
RUN dotnet publish /src/Services/Applicants.Api/applicants.api.csproj -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "applicants.api.dll"]