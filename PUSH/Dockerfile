#������� ����������� ��� ������ �������
FROM artifactory.domain.ru:PORT/dotnet/core/sdk:3.1 AS build

#����������� ������� ������� nuget �� artifactory
RUN dotnet nuget add source https://artifactory.domain.ru/api/nuget/nuget-remote -n artifactory && \
    dotnet nuget disable source nuget.org

#�������� �������� ����������, ��� ������� � artifactory
WORKDIR /etc/ssl/certs
ADD /certs/ /etc/ssl/certs

#��������� ������ �������
WORKDIR /home/*
COPY PUSH PUSH
RUN dotnet restore ./PUSH
RUN dotnet publish PUSH/*.MessageService.Web/*.MessageService.Web.csproj -c release -o /app --no-restore

#��������� ������
FROM artifactory.domain.ru:PORT/dotnet/core/aspnet:3.1
ENV ASPNETCORE_URLS="https://+"
ENV ASPNETCORE_Kestrel__Certificates__Default__Path="./cert-aspnetcore.pfx"
ENV ASPNETCORE_Kestrel__Certificates__Default__Password="YOUPASSWORD"
WORKDIR /app
COPY --from=build /app .
COPY cert-aspnetcore.pfx .
ENTRYPOINT ["dotnet", "*.MessageService.Web.dll"]