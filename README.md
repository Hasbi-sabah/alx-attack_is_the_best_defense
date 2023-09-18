# alx-attack_is_the_best_defense
A repo for the optional project Attack is the best defense

## ARP Spoofing and Sniffing Unencrypted Traffic

### Prerequisites
Before proceeding, ensure you have the required tools installed:

1. **Download the Program:**
   - Download the provided program:
     ```bash
     curl -O https://s3.amazonaws.com/intranet-projects-files/holbertonschool-sysadmin_devops/264/user_authenticating_into_server
     ```

2. **Install `tcpdump`:**
   - Update your package list:
     ```bash
     sudo apt-get update
     ```
   - Install `tcpdump`:
     ```bash
     sudo apt-get install tcpdump
     ```

3. **Install `tshark`:**
   - Install `tshark` for more human-readable output:
     ```bash
     sudo apt install tshark
     ```

### Intercepting Data
Now you're ready to intercept data between the `user_authenticating_into_server` program and the SendGrid server.

1. **Run `tcpdump`:**
   - Execute the following command to capture network traffic:
     ```bash
     sudo tcpdump -i eth0 -n -s 0 -A 'host smtp.sendgrid.net and port 587' -w sendgrid_traffic.pcap
     ```
     - `-i eth0`: Replace `eth0` with the network interface your program uses.
     - `-n`: Display numeric IP addresses and port numbers instead of resolving hostnames and services.
     - `-s 0`: Capture the entire packet.
     - `-A`: Display packet data (payload) in ASCII format.
     - `'host smtp.sendgrid.net and port 587'`: Capture traffic to and from SendGrid's SMTP server on port 587.
     - `-w sendgrid_traffic.pcap`: Save captured packets to a file for later analysis.

2. **Run Your Program:**
   - In a second Ubuntu terminal, give permission and then run your program:
     ```bash
     sudo chmod +x user_authenticating_into_server
     sudo ./user_authenticating_into_server
     ```

3. **Capture and Analyze:**
   - Wait for your program to execute, then interrupt the first terminal using `CTRL-C`.
   - You'll have a `sendgrid_traffic.pcap` file that you can analyze.

### Analyzing the Capture
You can analyze the capture file using either `tcpdump` or `tshark`:

- Using `tcpdump`:
   ```bash
   tcpdump -r sendgrid_traffic.pcap
   ```
- Using `tshark` for more human-readable output:
  ```bash
  15 3.999733 172.31.145.221 → 54.228.39.88 SMTP 80 C: User: bXlsb2dpbg==
  16 4.069955 54.228.39.88 → 172.31.145.221 TCP 66 587 → 57152 [ACK] Seq=196 Ack=63 Win=32256 Len=0 TSval=2327286244 TSecr=1468075274
  17 4.069956 54.228.39.88 → 172.31.145.221 SMTP 84 S: 334 UGFzc3dvcmQ6
  18 4.070087 172.31.145.221 → 54.228.39.88 TCP 66 57152 → 587 [ACK] Seq=63 Ack=214 Win=64128 Len=0 TSval=1468075345 TSecr=2327286244
  19 6.001628 172.31.145.221 → 54.228.39.88 SMTP 88 C: Pass: bXlwYXNzd29yZDk4OTgh
  ```
  
  This part of the output shows the authentication process with the information:
   
    User: bXlsb2dpbg==
  
    Pass: bXlwYXNzd29yZDk4OTgh

  Credentials are Base64 encoded, so the decoded information would be:
  
    User: mylogin
  
    Pass: mypassword9898!

