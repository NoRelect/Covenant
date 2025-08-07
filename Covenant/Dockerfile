FROM mcr.microsoft.com/dotnet/sdk:9.0 AS prepare-restore-files
ENV PATH="${PATH}:/root/.dotnet/tools"
RUN dotnet tool install --global --no-cache dotnet-subset
WORKDIR /app
COPY . ./
RUN dotnet subset restore "Covenant.sln" --root-directory /app --output /restore-subset/

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /app

COPY --from=prepare-restore-files /restore-subset .
RUN dotnet restore "Covenant.sln"

COPY . ./
RUN dotnet publish -c Release -o out --no-restore

FROM mcr.microsoft.com/dotnet/runtime:9.0 AS runtime
WORKDIR /app
COPY --from=build /app/out .
COPY ./Data ./Data
EXPOSE 7443
ENTRYPOINT ["dotnet", "Covenant.dll"]
