# TeamSpeak 6 Server - Beta Release README

Welcome to the Beta release of the TeamSpeak 6 Server! We're excited to have you onboard as you explore the next generation of TeamSpeak. This document will guide you through getting started and highlight important details about the current Beta.

As this is a Beta version, some features are still in development, and you may come across bugs. Your feedback is important and will help us shape a more stable and complete final release.

<h2><img width="24" src="/icons/teamspeak_blue.svg">&nbsp;About TeamSpeak</h2>

Tried & tested for nearly 25 years, we have a solution that fits everyone's needs. TeamSpeak offers the ideal voice communication for gaming, education and training, internal business communication, and staying in touch with friends and family. Our primary focus is delivering a solution that is easy to use, with high security standards, excellent voice quality, and low system and bandwidth usage.

## ℹ️ Beta Status & Known Issues
**This is a beta release. The main objective is to collect diverse feedback and identify potential issues before the stable version is launched.**

**Self-Hosted Server Files:** The dedicated server software for TeamSpeak 6 is still under active development and is not yet fully feature-complete. We are committed to delivering the complete TS6 experience for self-hosted servers.

**Feature Instability:** Some of the new features may be unstable or subject to change as we continue refining them.

**Feedback is Essential:** Your experience is invaluable, and your input is crucial to us. Please report any issues or share your suggestions in our [Community Forum](https://community.teamspeak.com/c/teamspeak-6-server/45) or directly here on [GitHub](https://github.com/teamspeak/teamspeak6-server/issues).

**Limitations:** Please note that **TeamSpeak 3 Server licenses are not compatible** with TeamSpeak 6 Servers, and currently, there is **no migration path available between the two versions**.

**Preview License:** In response to your feedback, the server now comes with a **temporary** 32-slot preview license to offer greater flexibility during the evaluation period. Please note that this license is **only valid for two months**.

Additionally, it is **not yet possible to obtain or upgrade to a larger license for TeamSpeak 6**.

We truly appreciate your patience and understanding as we continue to work on solutions to better support your needs in the future.

## 👇 Getting Started with TS6
To get started with the TeamSpeak 6 Client, please head over to our [Downloads](https://teamspeak.com/en/downloads/) page. 

For information on self-hosting, see the brief guide below. For the latest updates, announcements, and to engage with the TeamSpeak community, be sure to check out our [Community Forum](https://community.teamspeak.com/) and follow us on [Twitter](https://x.com/teamspeak).

Don't want to self-host, or simply require an easier way to get started with TeamSpeak 6? You can rent reliable and Official TeamSpeak 6 Servers directly through us at [TeamSpeak Communities](https://www.myteamspeak.com/communities).
## ⚙️ Configuration
### You can configure your server in three main ways:

1. **Command-Line Arguments** Pass options directly when starting the server (e.g., ./tsserver --default-voice-port 9987). This is useful for temporary changes or for scripting.

2. **Environment Variables:** Set environment variables before starting the server. This is useful for Docker or when you want to avoid command-line clutter.

3. **YAML Configuration File:** For a persistent configuration, it is highly recommended to use a tsserver.yaml file. You can generate a default config file using the --write-config-file flag.

Key settings you can control include network ports (voice, file transfer), database connections (supports SQLite and MariaDB), IP bindings, and logging options.

For a complete list of available options, run the server with the `--help` flag or refer to the [CONFIG.md](CONFIG.md).

### Running the Server with Binaries
If you are not using Docker, you can run the server directly on your operating system.

<h2><img width="22" src="/icons/linux.svg">&nbsp;On Linux:</h2>

Make the server binary executable:
```sh
chmod +x tsserver
```

Run the server from your terminal, making sure to accept the license:

```sh
./tsserver --accept-license
```

<h2><img width="22" src="/icons/windows.svg">&nbsp;On Windows:</h2>

Open Command Prompt or PowerShell and navigate to the directory where you extracted the server files.

Run the server executable, making sure to accept the license:
```powershell
tsserver.exe
```

<h2><img width="32" src="/icons/docker.svg">&nbsp;Running the Server with Docker (Recommended):</h2>
Docker is the easiest way to run the TeamSpeak 6 server in an isolated and manageable environment.

### 1. Simple docker run command:

For a quick start, you can use the docker run command.

```sh
docker run -it --rm \
  -p 9987:9987/udp \
  -p 30033:30033 \
  -e TSSERVER_LICENSE_ACCEPTED=accept \
  teamspeaksystems/teamspeak6-server:latest
```

### 2. Using docker-compose.yaml (for persistent setups):
This is the best practice for a server you intend to keep running. Create a docker-compose.yaml file:

```yaml
services:
  teamspeak:
    image: teamspeaksystems/teamspeak6-server:latest
    container_name: teamspeak-server
    restart: unless-stopped
    ports:
      - "9987:9987/udp"   # Voice Port
      - "30033:30033/tcp" # File Transfer
      # - "10080:10080/tcp" # Web Query
    environment:
      - TSSERVER_LICENSE_ACCEPTED=accept
    volumes:
      - teamspeak-data:/var/tsserver/

volumes:
  teamspeak-data:
```

## 🔗 Useful Links
[Official Website](https://teamspeak.com/en/)<br>
[Community Forum](https://community.teamspeak.com)<br>
[GitHub Issues](https://github.com/teamspeak/teamspeak6-server/issues)<br>

Your participation and feedback will help us shape the future of TeamSpeak! 💙<br>
Thank you for being a part of the TeamSpeak 6 Beta program and contributing to its progress! 🫡
