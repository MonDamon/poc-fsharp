#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /
COPY ["PocFsharp.fsproj", "PocFsharp/"]
RUN dotnet restore "PocFsharp/PocFsharp.fsproj"
COPY ["*", "PocFsharp/"]
WORKDIR "/PocFsharp"
RUN dotnet build "PocFsharp.fsproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "PocFsharp.fsproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "PocFsharp.dll"]