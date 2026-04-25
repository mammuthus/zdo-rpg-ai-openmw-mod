# Zdo RPG AI - OpenMW Mod

Integration of Zdo RPG AI system into OpenMW.

![Screenshot](./doc/screenshot.jpg)

# Instructions

## Simple deployment example

The expected setup is:

- the [Server](https://github.com/drzdo/zdo-rpg-ai) runs on a separate machine
- OpenMW runs on the game machine
- the [Client](https://github.com/drzdo/zdo-rpg-ai) runs on the game machine and bridges this mod to the server

1. Add the mod to `openmw.cfg`.

On macOS this file is usually:

```
~/Library/Preferences/openmw/openmw.cfg
```

Add these lines:

```ini
data="/path/to/zdo-rpg-ai-openmw-mod/zdorpgai"
content=zdorpgai.omwscripts
```

2. Create a client config file on the game machine.

Example:

```yaml
server:
	host: <SERVER_IP>
	port: 8080
	clientToken: "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef"

mod:
	provider: openmw
	openmw:
		dataDir: /path/to/zdo-rpg-ai-openmw-mod/zdorpgai
		logFilePath: /path/to/openmw.log
```

Notes:

- `host` and `port` must point to the running `zdo-rpg-ai` server
- `clientToken` must match `httpServer.clientToken` from the server config exactly
- `dataDir` must match the mod directory registered in `openmw.cfg`
- `logFilePath` must point to the real OpenMW log file on the game machine

3. Install .NET 9 SDK on the game machine.

`ZdoRpgAi.Client.Console` targets `net9.0`, so the machine that runs the client bridge needs a .NET 9 SDK or runtime that can run the client application.

If your distro does not provide `.NET 9` packages, install it with `dotnet-install`:

```sh
mkdir -p "$HOME/.dotnet"
curl -fsSL https://dot.net/v1/dotnet-install.sh -o /tmp/dotnet-install.sh
bash /tmp/dotnet-install.sh --channel 9.0 --install-dir "$HOME/.dotnet"
export PATH="$HOME/.dotnet:$PATH"
```

4. Run the client bridge from https://github.com/drzdo/zdo-rpg-ai.

Example:

```sh
dotnet run --project src/ZdoRpgAi.Client.Console -- --config /path/to/client-config.yaml
```

5. Start OpenMW and load the game.

The new resources from this mod should be visible in the Launcher window before you start the game.

<p>
	<img src="./doc/screenshot_2.jpg" alt="Launcher resources" width="49%" />
	<img src="./doc/screenshot_3.jpg" alt="OpenMW startup" width="49%" />
</p>

If everything is configured correctly:

- in the client log, you should see `Connected to server`
- in the client log, you should see `Connected to mod`
- in game, the expected confirmation is `ZdoRPG connected`
