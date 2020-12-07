FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-alpine AS base
WORKDIR /src
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine AS build
WORKDIR /src
COPY . .
RUN dotnet restore helloworld.csproj
RUN dotnet build "./helloworld.csproj" -c Debug -o /out

FROM build AS publish
RUN dotnet publish helloworld.csproj -c Debug -o /out

# Building final image used in running container
FROM base AS final
WORKDIR /src
COPY --from=publish /out .
ENV ASPNETCORE_URLS=http://*:8080

RUN apk add --no-cache \
        ca-certificates \
        \
        strace

# ENTRYPOINT ["strace", "-f", "-o", "/tmp/outfile.txt", "dotnet", "helloworld.dll"]
ENTRYPOINT ["dotnet", "helloworld.dll"]