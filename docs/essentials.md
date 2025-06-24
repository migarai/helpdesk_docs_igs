---
sidebar_position: 3
---
# Essentials
Other Docs
<!-- * [New Setup](/newSetup.md)
* [Issues](/issues.md)
* [Documenting](/documenting.md) -->

## ClickUp For Different Regions

 * UK/ UZ - [https://app.clickup.com/9018848552/v/l/6-901805430284-1](https://app.clickup.com/9018848552/v/l/6-901805430284-1)
 * AUS/ NZ - [https://app.clickup.com/31195628/v/l/xr0fc-103](https://app.clickup.com/31195628/v/l/xr0fc-103)

## RMS Login
 * For  the **AUS/ NZ** sites, use your personal Teltonica Account.
 * For the UK/ US sites, Use the credentials below.
    ```text
     Username:  team.mw@i360.lk
     Password: MW@i360.lk
    ```
## RMS Id
This is the Id of the Router that Related to the RPI.
 1. Go to **[Teltonika](https://rms.teltonika-networks.com/management/devices)**. Login using personal account for AUS related projects, use this for other.
    username: `team.mw@i360.lk`
    password: `MW@i360.lk`

 2. Find the router related to the site and go to device details.
   ![Router Details](\images\teltonika.router.png)

 3. In the page **URL**, last segmant use as the **RMS Id**.
   ![RMS ID](/images/rmsid.png)
   RMS ID: `1550528`

## Default IP Distribution.
 | Device                              | Type    | IP              |
 | ----------------------------------- | ------- | --------------- |
 | Face Recognitions `FR`              | entry 1 | `192.168.1.165` |
 |                                     | exit 1  | `192.168.1.166` |
 |                                     | entry 2 | `192.168.1.167` |
 |                                     | exit 2  | `192.168.1.168` |
 | Hikckvision Access Controller `HAC` | hac 1   | `192.168.1.175` |
 |                                     | hac 2   | `192.168.1.176` |
 | Port forwarding IP                  |         | `127.0.0.1`     |
 | Raspberry Pi `RPI`                  | RPI 1   | `192.168.1.150` |

## Getting Employee Id
 Get the **Employee** Id from the URL last segment.
 1. Go to the portal of the relavent site.
 2. Go to `Projects`.
 3. Search and find the related project.
 4. In the Project, in **Site Assignments** Section, go to `Site Assignments`. This shows all the employees assigned to the project.
 5. Search and find the related employee. and view it.
   ![Employee Id](/images/client.employeeid.png)
   Ex: employeeId = `66d80ed5d0171b465377afd3`

## Open VPN
 1. Login to the **OpenVPN CloudConnexa** - [https://myaccount.openvpn.com/signin/cvpn/infinitum360](https://myaccount.openvpn.com/signin/cvpn/infinitum360)
 2. Go to `Hosts`>> `Hosts`. This shows all the clients.
 3. Serach and find the related client. If not exists create a new one.
   ![CloudConnet](/images/openvpn.hosts.png)
 4. Go to `Connectors` >> `Add Connector`
   Name: provide related site name (lowercase and no space).
   Clck `Deploy` >> `Deploy Connector`
 5. The last segmant of the **URL** is the **Open VPN ID**.
   ![Open VPN Id](/images/opnevpn.openvpnid.png)
   <span style={{ color: "red" }}>Only get the last part after the '/'.</span>

## RPI Login
 username: `pi`
 password:
 ```text
 !Es4VIDfa&BzrPhB@iUMDGKZ92k%@z&ZmYkJMo#x2
 ```

## RPI Commands

### Restart Docker
Restarting the docker in RPI. <span style={{ color: "red" }}>must be on `/client` </span>
```bash
docker-compose restart
```
### Stop Docker
Stop the running docker instacne. <span style={{ color: "red" }}>must be on `/client` </span>
```bash
docker-compose down
```
### Start Docker instance
```bash
docker-compose up -d
```
### AWS Login
 ```bash
  aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 312466207882.dkr.ecr.ap-south-1.amazonaws.com
 ```
## Configurations

### RPI Hostname
1. SSH to RPI
2. If the RPI host name is "new-setup", change the name by
   ```bash
    sudo raspi-config
   ```
   This will open **RaspberryPi Configuration tool**.
3. Go to `System options` >> `Hostname`. Provide site name.
4. This will restart the RPI.


### OpenVPN and RPI
1. Login to RPI
2. `cd` to `client`.
3. Open previous OpenVPN configuration `client.conf`
   ```bash
    sudo nano /etc/openvpn/client.conf
   ```
4. Get the new configuration file.
  i. Go to [OpenVPN CloudConnexa](https://myaccount.openvpn.com/signin/cvpn/infinitum360)
  ii. `Hosts` >> `Hosts` >> Select the client >> `Connectors` >>  Select the connector >> `Deploy` >> `Download Profile`
  iii. Copy the contetn in the file.
5. BackUp the old `client.conf`.
   ```bash
     sudo mv /etc/openvpn/client.conf /etc/openvpn/client.conf.back
   ```
6. Create a new `client.conf`. Past the content (step 4)
   ```bash
    sudo nano /etc/openvpn/client.conf
   ```
   save and exit
7. Restart the **openvpn service**
   ```bash
    sudo systemctl restart openvpn
   ```
   <span style={{ color: "red" }}> Run this for at least 2 times </span>
8. After few second the connector should be online in CloudConnexa.
   ![OpenVpn online](/images/openvpn.online.png)

### VPS
<!-- Is this to copy creds to the RPI -->
1. Open **Remote Desktop Connection**.
2. Login Credentials
   ```text
    address: 65.0.103.184
    username: DEVELOPER\Administrator
    password: otIQMuwhCV7q3g;gXg7Ov9VcNUVj@P@s
   ```
3. Connect to OpenVPN if disconnected.
4. Go to (Create if not exists) the folder related to client in Desktop.
5. Go to (Create if not exists) the folder related to the site.
6. Get the credentials from the related thing
   ![Thing Creds](/images/client.getthingcreds.png)
   copy the "example.zip" file to the VPS.
7. Extract the "example.zip" to `\creds` folder.
8. Open the folder root in **PowerShell**. (hold `shift` + `right click`).
9. Copy credentials to the RPI. Use default **RPI Password**.
    ```bash
     scp -r .\creds\* pi@100.96.8.246:~\client\creds
    ```
    \* If command didn't run check OpenVpn CloudConnexa Connector is online.
    \* <span style={{ color: "red" }}>Replace the IP with OpenVPN Connector IP.</span>
    ![CloudeConnexa Connector](/images/openvpn.online.png)
10. Open **PuTTY** in **VPS** .
11. Load a previous profile. Give a new name (`"client_name" - "site_name"`).
12. Change the IP address to Connecter IP.
13. Save and Open Connection.
14. Enter **RPI configuration Tool**.
    ```bash
     sudo raspi-config
    ```
15. `System Options` >> `Hostname` >> "site_name"

### Ngrok Remote Connection
Check the RPI has the **Ngrok** by,
```bash
 ngrok -v
```
IF not installed Install it using,
```bash
 npm install -g ngrok
```
In case above command didn't work out, try the other installation methods listed [here.](#ngrok-installation)
To use static URLs, A <span style={{ color: "red"}}>Ngrok account with a card</span> is needed.
Get the **Authtoken** from [**here**](https://dashboard.ngrok.com/get-started/your-authtoken). Make suer <span style={{ color: "red"}}>**the authtoken is visible**</span>. befor entering the command.
```bash
 ngrok config add-authtoken $YOUR_AUTHTOKEN
```
Replace `$YOUR_AUTHTOKEN`

1. Start **port forwarding**.
   ```bash
    ngrok tcp 22
   ```
2. This will returns a output like this,
   ![Ngrok](/images/rpi.ngrok.png)
   copy these for further steps
   address: *0.tcp.ap.ngrok.io*
   port: *14465*
3. By pasting address and port in **winSCP**. You can start the **remote connection**.
   ![WinSCP Ngrok](/images/winscp.ngrok.png)



### env Config
1. Login to RPI.
2. `cd` to `/client`
3. Open `.env`
   ```bash
    nano .env
   ```
4. Make these changes.
   `CLIENT_ID` = Thing Id. (*ex: A376_NwxIniFNW0*)
   `KEY_NAME` = `private.pem.key` (remove "id-")
   `CRET_NAME` = `device.certificate.pem.crt` (replace "id-" with "device.")
   `TAG` = prodV2K5 (add "5")
   `TIMEZONE` = use correct time zone ([time zones](#time-zones))
   `CLIENT` = `igs`
   `PROJECT` = name ofthe project

5. Double check, save and exit.


## Time Zones

### DST Configurations

## SADP
SADP (Search Active Device Protocol) is a tool used for detecting, configuring, and managing Hikvision network devices (such as IP cameras, NVRs, and DVRs) on a local network. ([download](https://www.hikvision.com/en/support/tools/hitools/clea8b3e4ea7da90a9/))
![SADP](/images/sadp.png)
**Adminstrator Password**: `Hik12345`
Change the IPs according to the [default configuration](#default-ip-distribution).

* Copy the `Device Serial No.` to **ClickUp document**.
* The device configuration should be likem this,
  ![Device Configuration](/images/sadp.configuration.png)
* Refresh and make sure changes are applied.

## iVMS-4200 {#ivms-4200}
iVMS-4200 is a video surveillance management software developed by Hikvision. It is used to monitor, control, and manage Hikvision security devices.([download](https://www.hikvision.com/content/dam/hikvision/en/support/download/vms/ivms4200-series/software-download/4200-3-11-1-11/iVMS-4200V3.11.1.7_E.exe))

*  If a **used device** is need to reconfigure.
   `settings` >> `System` >> `Maintenance` >> In "Restore Parameters" `Restore All`
   ![iVMS Settings](/images/ivms.settings.png)
   This will reset the device. use [SADP](#sadp) to reassign IPs.

**Adding devices to iVMS**
1. Open iVMS.
2. `Device Managment` >> `Device` >> `Add`
   ![IVMS](/images/ivms.png)
3. Fill the fileds
   * `Name`: any name ex:*Fr165*
   * `Address`: ip related to the device (for FR: `192.168.1.165`)
   * `username`: `admin`
   * `password`: `Hik12345`
4. Add the other devices as above.

**In case if [SADP](#sadp) unable to activate the devices, It can be also done with iVMS**

1. Click `Online Device`. This shows available devices within network.
2. Choose the "inactive" device and click on "Active".
  `username`: `admin`
  `password`: `Hik12345`
  If asked for an email,
  ![FR Email](/images/fr.email.png)
   `matheesha@i360.lk`
3. Change the IPs according to [default distribution](#default-ip-distribution) by clicking `net stats`.
   ![iVMS Operations](/images/ivms.netwrk.png)

**FR Settings**
1. Select the device want to configure.
2. `settings` >> `System` >> `System Settings` >> `Time Settings`
3. Change "Time Zone" according to the project.
   `Time Sync`: NTP
   `Server Address`: `time.google.com`
   `port`: `123`
   `Interval`: `60`
4. Save and go to `DST`.
   `Enable DST`: True
   Check [Time/ Daylight Saving Configurations](#dst-configurations) and fill accordingly.
5. Save and go to `General`.
   `Recognition Interval`: `4`
   `Authentication Interval`: `4`
6. Save and go to `Access Control`
   `Open Duration`: `1`
7. Save and go to `Samrt`.
   `Live Face Detection Security Level`: `1`
   `Recognition Distance`: `1m`
   `Fase with Mask Detection`: `False`
8. Save exit
Check and Configure each **FR** accordingly

**HAC Settings**
* Update the **ClickUp** with device serial, firmware versions.
1. `System` >> `Time` >> Under the 'Time Zone'
   `Time Zone`: Change accordingly
   `NTP`: True
   `Server Address`: `time.google.com`
   `NTP Port`: 123
   `Sync Interval`: 60
   Save
2. Configure DST if applied.
3. `Home` >> `Access Control` >> select the HAC
   ![iVMS HAC](/images/ivms.hac.png)
  i. For each door,
     `Open Duration: 3`
  ii. Exapnd the each door to get entrance/ exit settings.
    ![Entence/ Exit](/images/ivms.hac.ancat.png)
    Go to `Advanced`,
    * **Entrnace** => **Cathode** should be this.
      ![HAC Cathode](/images/ivms.hac.advanced.png)
    * **Exit** => **Anode** should be this.
      ![HAC Anode](/images/ivms.hac.advanced.an.png)

  iii. `Authorization` >> `Access Group` >> `Add`
    define a test user for testing. If you havent created a user before go to
    `Name`: any name ex: *test user*

## Get IoT Credentials

## Using Ngrok and WinSCP
1. Download [WinSCP](https://sourceforge.net/projects/winscp/).
2. SSH to RPI.
3. Install **Ngrok** in **RPI**.
   ```bash
    sudo npm install -g ngrok
   ```
   If above installation doesn't work. Use a compatible installation method from [here.](#ngrok-installation).
4. After installing ngrok, config it by getting **auth token** from [here](https://dashboard.ngrok.com/get-started/your-authtoken)
   ```bash
    ngrok config add-authtoken 2lzztjZonSd85bbS1a138CUveWB_5B4DgAP4jH4UUFKFnwuuf
   ```
5. start server by
   ```bash
    ngrok tcp 22
   ```
6. from here,
   ![Ngrok Server](/images/ngrok.server.png)
   Seperatly copy the **link** and **port**.
   ex: link: *0.tcp.au.ngrok.io*
   ex: port: *14595*
7. Go to **WinSCP**.
   ![WinSCP](/images/winscp.log.png)
   `Host name`: paste the link.
   `Port number`: paste the port.
   `User name`: hostname of the RPI.
   `Password`: RPI password
8. After logged in you will able to access files in **local at left** and **RPI at rigth**.
   ![WinSCP Interface](/images/winscp.in.png)
   You can now config the **RPI** from here.
## Other

### Ngrok Installation
```bash
curl -sSL https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
| sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \
&& echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
| sudo tee /etc/apt/sources.list.d/ngrok.list \
&& sudo apt update \
&& sudo apt install ngrok
```
<span style={{ color: "red" }}>If above solution also didn't work out. </span>

```bash
 $ nvm install 19
 $ nvm -v
 $ nvm ls
 $ nvm use 19.xx.xx
 $ nvm install -g yarn ngrok
```
