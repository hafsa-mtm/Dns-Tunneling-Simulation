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
###The attacker machine (Kali Linux) uses dnschef, a rogue DNS proxy, to intercept DNS queries from the victim.
  -Purpose: Redirect DNS queries for a fake domain (e.g., *.tunnel.local) to the attacker's IP, enabling covert C2 communication.
  
  ![image](https://github.com/user-attachments/assets/0b114df3-97b7-4f8c-bc33-74e2ead71348)

  -Domain Used: tunnel.local (custom pseudo-domain)
  
  -Listener Port: 53 (standard DNS)

  -Behavior: Replies to DNS queries with the attacker's IP instead of resolving them, enabling the victim to unknowingly connect to the dnscat2 server.
- `/malware`: Contains the Ruby-based malware script
- `/dnscat2`: Config files and notes on the client/server interaction
- `/Autorun`:
- `/data_exfiltration`: Sample files used in exfiltration
- `/screenshots`: Execution flow screenshots

## How to Run (For Educational Use Only)

> ⚠️ Warning: This is for **educational and testing purposes** only. Do not use in unauthorized environments.

1. Start `dnschef` with a fake domain.
2. Launch `dnscat2-server`.
3. Place the `autorun.desktop` on the target and simulate execution.
4. The Ruby payload connects back to the `dnscat2` server.
5. You can exfiltrate data or run commands from the server.

## Legal Notice

This project is strictly for **educational purposes** and **authorized testing labs**. Do not deploy in live or unauthorized environments.
