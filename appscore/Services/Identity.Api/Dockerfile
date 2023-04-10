FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
COPY Services/Identity.Api/Identity.Api.csproj /src/Services/Identity.Api/
COPY Foundation/Events/Events.csproj /src/Foundation/Events/
RUN ls -la
RUN dotnet restore /src/Services/Identity.Api/Identity.Api.csproj 
COPY Services/Identity.Api /src/Services/Identity.Api/
COPY Foundation/Events /src/Foundation/Events/
RUN cd /src/Services/Identity.Api && dotnet build Identity.Api.csproj -c Release -o /app/build

FROM build AS publish
RUN dotnet publish /src/Services/Identity.Api/Identity.Api.csproj -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Identity.Api.dll"]