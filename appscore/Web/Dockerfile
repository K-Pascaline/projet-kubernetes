FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
COPY Web/Web.csproj /src/Web/
COPY Foundation/Http/Http.csproj /src/Foundation/Http/

RUN dotnet restore /src/Web/Web.csproj 
COPY Web /src/Web/
COPY Foundation/Http /src/Foundation/Http/
RUN cd /src/Web && dotnet build Web.csproj -c Release -o /app/build

FROM build AS publish
RUN dotnet publish /src/Web/Web.csproj -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Web.dll"]