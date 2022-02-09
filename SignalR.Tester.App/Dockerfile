FROM mcr.microsoft.com/dotnet/runtime:5.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["SignalR.Tester.App/SignalR.Tester.App.csproj", "SignalR.Tester.App/"]
COPY ["SignalR.Tester.Core/SignalR.Tester.Core.csproj", "SignalR.Tester.Core/"]
COPY ["SignalR.Tester.Utils/SignalR.Tester.Utils.csproj", "SignalR.Tester.Utils/"]
RUN dotnet restore "SignalR.Tester.App/SignalR.Tester.App.csproj"
COPY . .
WORKDIR "/src/SignalR.Tester.App"
RUN dotnet build "SignalR.Tester.App.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SignalR.Tester.App.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
#Agents folder should have agent
COPY ["Agents/","Agents/."]
ENTRYPOINT ["dotnet", "SignalR.Tester.App.dll"]
