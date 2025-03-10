# Check if your tunnel/port forward is active and running:

## Command line:
- netstat -nao
- route print
- netsh interface portproxy show all

## PowerShell
- Get-NetIPConfiguration
- Test-Connection -ComputerName <remote_ip_or_hostname> -Source <tunnel_interface_ip>
# Tools for Creating a Port Forward or Tunnel on Windows:

## OpenSSH:
```
ssh -L <local_port>:<remote_address>:<remote_port> <username>@<remote_host>
```

## Proxy Binaries for Windows
Windows 10 has SSH (Thanks WSL!)
plink.exe (In Kali)

## Dymanic Port Forwarding

`ssh -N -D 127.0.0.1:1337 user@remotehost -p 22222`

The `-D` means dynamic. The `1337` is the port you proxy traffic through. With this you can setup a Socks proxy on your local machine to send traffic to port `1337`, thus said traffic will be sent through that port, and then through the remote host. `-N` is optional, it means do not execute a remote command. Specifying `127.0.0.1` is also optional. It'll default to `127.0.0.1`.

Via Putty:

![PT](PT.png)

## Port Forwarding

**Local port forwarding**

`ssh -N -L 0.0.0.0:4455:10.1.1.1:445 user@remotehost`

or

`ssh -L 8080:localhost:8080 user@remotehost`

The -L stands for local. First 8080 is port of your machine, 2nd 8080 is port of remote machine (doesn't have to be the same port). After successful ssh connection, if you request localhost:8080 (like from your browser, if you're trying to access a localling listening web server on the remote machine) that request will be send through that ssh tunnel to that remote host and you will be able to connect to that remote service. 
In other words, what this tunnel does is whatever request you sent to your localhost it will be forwarded to remote localhost.

**Remote port forwarding**
 `ssh -N -R 10.10.1.1:4455:127.0.0.1:445 attacker@10.10.1.1`

**Socks5 with SSH**
 `ssh -N -D 127.0.0.1:8888 admin@10.1.1.1`

## Plink:
Source: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
Plink Binary Located in Kali Linux: /usr/share/windows-binaries
```
plink -L <local_port>:<remote_address>:<remote_port> <username>@<remote_host>
```

### Plink Port Forwarder on Windows:

plink in an interactive shell

```
plink.exe -ssh -L 0.0.0.0:4444:10.1.1.1:445 admin@10.10.1.1
plink.exe -ssh -R 10.10.1.1:4444:127.0.0.1:445 attacker@10.10.1.1
plink.exe -ssh -D 127.0.0.1:8080 admin@10.10.1.1
```

plink in non interactive shell

```
cmd.exe /c echo y | plink.exe -ssh -l admin -pw password -R 10.10.1.1:4444:127.0.0.1:445 attacker@10.10.1.1
```

netsh local port forwarding:

```
netsh interface portproxy add v4tov4 listenaddress=10.1.1.1 listenport:4444 connectaddress:10.1.1.1 connectport:3306
netsh advfirewall firewall add rule name="4444_to_3306" protocol=TCP dir=in localip=127.0.0.1 localport=3306 action=allow
```

From <https://werebug.com/pentest-cheatsheet/#port-forwarding>

# Netsh:

```
netsh interface portproxy add v4tov4 listenaddress= listenport= connectaddress= connectport= protocol=tcp

# Context:
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=443 connectaddress=172.21.0.0 connectport=443
# Verify port forward was created:
netsh interface portproxy show all
# Remove port forward
netsh interface portproxy delete v4tov4 listenaddress=0.0.0.0 listenport=443
```

# Proxifier

## 1. Download and Install Proxifier
- Go to the [Proxifier website](https://www.proxifier.com/download/) and download the latest version of Proxifier.
- Install Proxifier by following the installation wizard.

## 2. Open Proxifier
- Launch Proxifier from the Start menu or desktop shortcut.

## 3. Configure a Proxy Server
- Go to `Profile` > `Proxy Servers...`
- Click `Add` to add a new proxy server.
- Enter the details of the proxy server you want to use:
  - **Address:** The IP address or hostname of the proxy server.
  - **Port:** The port number of the proxy server.
  - **Protocol:** Choose the protocol (SOCKS 4, SOCKS 5, or HTTPS).
  - If the proxy server requires authentication, check the `Enable` box under `Authentication` and enter your username and password.
- Click `OK` to add the proxy server.

## 4. Create a Proxification Rule
- Go to `Profile` > `Proxification Rules...`
- Click `Add` to create a new rule.
- In the `Rule` dialog:
  - **Name:** Give your rule a descriptive name.
  - **Applications:** Specify the applications that should use the tunnel. You can leave this as `Any` to apply to all applications.
  - **Target Hosts:** Specify the target hosts. You can leave this as `Any` to apply to all destinations.
  - **Action:** Choose the proxy server you configured earlier.
- Click `OK` to create the rule.

## 5. Verify the Configuration
- Ensure that the rule you created is listed and enabled in the Proxification Rules window.
- Click `OK` to save and apply the rules.

## Example Configuration
``` 
Proxy Server Configuration
- **Address:** `proxy.example.com`
- **Port:** `1080`
- **Protocol:** `SOCKS 5`
- **Authentication:** Enabled (if required)
  - **Username:** `your_username`
  - **Password:** `your_password`

## Proxification Rule Configuration
- **Name:** `My Tunnel`
- **Applications:** `Any`
- **Target Hosts:** `Any`
- **Action:** Use the proxy server `proxy.example.com`
```

# Chisel

## Step 1: Download Chisel
1. Source: [Chisel releases page](https://github.com/jpillora/chisel/releases).
2. Download the appropriate version for Windows (e.g., `chisel-windows-amd64.exe`).

## Step 2: Set Up the Chisel Server (On the Remote Host)
1. Place the `chisel` executable on your remote server.
2. Open a terminal on the remote server.
3. Start the Chisel server with the following command:

    ```bash
    ./chisel server --reverse --port <server_port>
    ```

    - Replace `<server_port>` with the port you want Chisel to listen on (e.g., `8000`).

    Example:

    ```bash
    ./chisel server --reverse --port 8000
    ```

## Step 3: Configuring Chisel Client for Windows
1. Place the downloaded `chisel-windows-amd64.exe` executable on your Windows machine.
2. Open Command Prompt or PowerShell.
3. Run the Chisel client with the following command:

    ```powershell
    chisel-windows-amd64.exe client <server_address>:<server_port> R:<local_port>:<remote_address>:<remote_port>
    ```

    - Replace `<server_address>` with the IP address or hostname of your remote server.
    - Replace `<server_port>` with the port the Chisel server is listening on.
    - Replace `<local_port>` with the local port you want to use.
    - Replace `<remote_address>` with the target address you want to access through the tunnel.
    - Replace `<remote_port>` with the target port you want to access through the tunnel.

    Example:

    ```powershell
    chisel-windows-amd64.exe client 172.21.0.0:8000 R:8080:localhost:80
    ```

    This command forwards local port `8080` to `localhost:80` on the remote server.

# Ligolo-ng

## Step 1: Download Ligolo-ng
1. Go to the [Ligolo-ng releases page](https://github.com/tnpitsecurity/ligolo-ng/releases).
2. Download the appropriate version for Windows (e.g., `ligolo-ng-agent-windows-amd64.exe` and `ligolo-ng_proxy_X.X-alpha_windows_amd64.zip`).
3. If we are running the ligolo proxy on kali linux we can pull it from the Kali Repo:
```
sudo apt install ligolo-ng
```
4. Download the Wintun driver (https://www.wintun.net/) and place the wintun.dll in the same folder as Ligolo (make sure you use the right architecture).

## Step 2: Set Up the Ligolo-ng Proxy Server
1. Create a tunnel interface: 
```
sudo ip tuntap add user [your_username] mode tun ligolo
sudo ip route add 172.21.0.0/24 dev ligolo
sudo ip link set ligolo up
```

For newer versions >= v0.6 we can use interface_create command to have ligolo create a new interface for us. 

## Step 3: Starting the ligolo-ng proxy server
1. In Kali Linux: 
```
$ ligolo-proxy -h # Help options
$ ligolo-proxy -autocert # Automatically request LetsEncrypt certificates
$ ligolo-proxy -selfcert -laddr 0.0.0.0:443 # Use self-signed certificates with custom local address and port
```

## Step 4: Initialize Ligolo agent on Windows: 
1. Place the downloaded `ligolo-ng-agent-windows-amd64.exe` executable on your Windows machine.
2. Open Command Prompt or PowerShell.
3. Run the Ligolo-ng agent with the following command:
```
ligolo-ng-agent-windows-amd64.exe -connect 172.21.0.0:443
ligolo-ng-agent-windows-amd64.exe -connect 172.21.0.0:443 -ignore-cert # This is dangerous!
```
4. Once the agent is running you should see a notification from the proxy: 
```
INFO[0104] Agent joined. name=afsimmons@ntworkstation remote="XX.XX.XX.XX:36000"
```

## Step 5: Start the tunnel: 
1. Use the session command to select the agent: 
```
ligolo-ng » session 
? Specify a session : 1 - afsimmons@ntworkstation - XX.XX.XX.XX:36000
```
2. Start the session
```
tunnel_start --tun ligolo
INFO[0690] Starting tunnel to afsimmons@ntworkstation   
```

## Step 6 (Optional):
In case your windows route is not working correctly you can add a route to call to the proxy by using the following commands: 
```
> netsh int ipv4 show interfaces

Idx     Met        MTU          State                Name
---  ----------  ----------  ------------  ---------------------------
 20           25       65535  connected     ligolo
   
> route add 192.168.0.0 mask 255.255.255.0 0.0.0.0 if [THE INTERFACE IDX]
```
