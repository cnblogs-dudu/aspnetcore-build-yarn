FROM microsoft/dotnet:2.1-sdk

# set up environment
ENV ASPNETCORE_URLS http://+:80
ENV ASPNETCORE_PKG_VERSION 2.1.1

# set up node
ENV NODE_VERSION 10.6.0
ENV YARN_VERSION 1.7.0
ENV NODE_DOWNLOAD_SHA c9be65055b9492bad9539acdf31a37f75785f1ad58b6659166233b520a4a9290
ENV NODE_DOWNLOAD_URL https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz

RUN wget "$NODE_DOWNLOAD_URL" -O nodejs.tar.gz \
    && echo "$NODE_DOWNLOAD_SHA  nodejs.tar.gz" | sha256sum -c - \
    && tar -xzf "nodejs.tar.gz" -C /usr/local --strip-components=1 \
    && rm nodejs.tar.gz \
    && npm i -g yarn@$YARN_VERSION \
    && ln -s /usr/local/bin/node /usr/local/bin/nodejs

# warmup NuGet package cache
COPY packagescache.csproj /tmp/warmup/
RUN dotnet restore /tmp/warmup/packagescache.csproj \
      --source https://api.nuget.org/v3/index.json \
      --verbosity quiet \
    && rm -rf /tmp/warmup/

WORKDIR /
