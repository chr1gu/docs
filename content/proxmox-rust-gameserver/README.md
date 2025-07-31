# Run Rust Server in Proxmox with LinuxGSM

## Customize Rustserver

1. Run `./rustserver details` to find the config file locations.
2. Copy the settings from `./lgsm/config-lgsm/rustserver/_default.cfg` to `./lgsm/config-lgsm/rustserver/rustserver.cfg` to customize it
3. Edit the lgsm config: `vim ./lgsm/config-lgsm/rustserver/rustserver.cfg`
   Change the RCON password and server name
4. Edit the server config and change description, etc.: `vim ./serverfiles/server/rustserver/cfg/server.cfg`
   * Disable decay: `decay.scale 0`
5. Login to http://facepunch.github.io/webrcon/#/<YOUR IP>:28016/info
6. Go to Console use server commands: https://rust.fandom.com/wiki/Server_Commands
   1. Create Owner: `global.ownerid "SteamID" "_chrigu" "because I can"`
   2. Disconnect from server and re-connect to have admin privileges
