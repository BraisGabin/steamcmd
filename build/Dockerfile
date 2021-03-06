FROM debian:jessie
MAINTAINER DST Academy <support@dst.academy>

# Install dependencies.
RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		ca-certificates \
		curl \
		lib32gcc1 \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

# Install gosu.
RUN gpg --keyserver pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
	&& curl -o /usr/local/bin/gosu -fsSL "https://github.com/tianon/gosu/releases/download/1.9/gosu-$(dpkg --print-architecture)" \
	&& curl -o /usr/local/bin/gosu.asc -fsSL "https://github.com/tianon/gosu/releases/download/1.9/gosu-$(dpkg --print-architecture).asc" \
	&& gpg --verify /usr/local/bin/gosu.asc \
	&& rm /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu

# Define environment variables.
ENV STEAM_USER="steam" \
	STEAM_HOME="/opt/steam"

# Add steam user.
RUN adduser --disabled-login --gecos "Steam" --home $STEAM_HOME $STEAM_USER

# Install SteamCMD.
RUN curl -fsSL "https://cdn.steamstatic.com/client/installer/steamcmd_linux.tar.gz" | gosu $STEAM_USER tar -xzvC $STEAM_HOME \
	&& gosu $STEAM_USER $STEAM_HOME/steamcmd.sh \
		+@ShutdownOnFailedCommand 1 \
		+quit \
	&& rm -rf $STEAM_HOME/Steam/logs $STEAM_HOME/Steam/appcache/httpcache \
	&& find $STEAM_HOME/package -type f ! -name "steam_cmd_linux.installed" ! -name "steam_cmd_linux.manifest" -delete

# Copy common scripts.
COPY /script/* /usr/local/bin/

# Set default command.
CMD ["steamcmd"]
