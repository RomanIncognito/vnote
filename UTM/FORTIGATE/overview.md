# overview
Modes of operation
1) NAT Layer 3 router. Routed by ip. Configured per VDOM
2) Transparent. Layer 2 switch of bridge.  NO IPs. Only forward. Also configured per VDOM (virtials domain).

Management port: 192.168.1.99/24
User: admin  Password: (blank)

Update location. update.fortiguard.net 443
we filtering, dns filtering, antispam: 
- service.fortiguard.net 53 8888
- ...........................

commands:
-get system status
-show system interface <portx>
show full-configuration system interface port3
-execute shutdown
set mode static
show system admin  # show all administrators


Root CA certificate must be imported into the client machine MANUALY