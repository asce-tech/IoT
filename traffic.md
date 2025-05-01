Network segemntation: 
- Use your home router or firewall (pfSense, UFW) to create a separate VLAN/subnet for IoT devices.
```bash
# On pfSense: Create VLAN 20 for IoT
# Assign DHCP range like 192.168.20.0/24
```
- Restrict communication between the IoT VLAN and critical network segments.
  - Create VLANs
    - IoT VLAN → `192.168.20.0/24`
    - Internal LAN (Workstations) → `192.168.1.0/24`
  - Set Interface Rules (on pfSense or similar)
     - Go to Firewall > Rules > IoT VLAN and add rules:
        - Block access to the internal network
          ```bash
          Action: Block
          Protocol: Any
          Source: IoT VLAN net (192.168.20.0/24)
          Destination: Internal LAN net (192.168.1.0/24)
          Description: Block IoT to LAN
          ```
        - Allow Access to the internet only:
          ```bash
          Action: Pass
          Protocol: Any
          Source: IoT VLAN net
          Destination: any
          Description: Allow IoT to Internet
          ```
  - No Rules on LAN to IoT
    - Don’t create rules from LAN to IoT unless required. This ensures one-way isolation.


- Real-time Traffic Analysis
  - How to Implement: Deploy tools like Wireshark, Suricata, or Snort to monitor packet-level activity.
  - Example Snort config:
  ```bash
  snort -i eth0 -c /etc/snort/snort.conf -A console
  ```
  - Log and analyze packets from the IoT VLAN.
 
- Centralized Logging + SIEM Integration:
- Configure IoT devices or a gateway/router to send logs to a central syslog server.
- Use Filebeat or Logstash to ingest logs into Elasticsearch.
- Visualize logs in Kibana in `.yaml` file
  
        
