FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS tools
RUN dotnet tool install --tool-path /tools dotnet-monitor --add-source https://dnceng.pkgs.visualstudio.com/public/_packaging/dotnet5-transport/nuget/v3/index.json --version 5.0.0-preview.20251.1

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS runtime

COPY --from=tools /tools /tools
ENV PATH="/tools:${PATH}"

ENV COMPlus_EnableDiagnostics="0"

WORKDIR /tools
ENTRYPOINT ["dotnet-monitor", "collect"]