# Capture SolarEdge

To extract the SolarEdge encryption key make sure to run tcpdump before the Inverter is connected to Internet. On the captured file(s) run command:

```bash
python find-key-in-pcap.py /<path>/*.pcap
```

Save the key in a text file.
Next, install the logging database:

```bash
sudo apt install mariadb-server
sudo apt install python-mysql
sudo apt install python-crypto
mkdir /opt/se-logger
```

Create the required tables and user to access it.

```sql
$ mysql -u root
> CREATE USER 'selogger'@'localhost' IDENTIFIED BY 'Wielingen8';
> USE solaredge;
> GRANT ALL ON solaredge TO 'selogger'@'localhost';
> EXIT;
```

Copy the SE Logger files to the _/opt/se-logger_ directory.

```bash
mv /tmp/liveupdate.py /opt/se-logger/.
mv /tmp/se-logger-service.sh /opt/se-logger/.
mv /tmp/find-key-in-pcap.py /opt/se-logger/.
```

Edit _liveupdate.py_ and enter the username and password of the MySQL database in the **SETTINGS** section og the script. Also, rnter the encryption key extracted from the capture.

```py
...
# SETTINGS
inverter_private_key = '\x12\x98\x13\x41\x23\xb6\x2d\x5b\x15\x1b\x6f\x65\xd2\xc5\x05\x5a'
db_user = "sdlogger"
db_pass = "Wielingen8"
db_name = "solaredge"
db_host = "localhost"
...
```

Open 'se-logger-service.sh' en zorg dat in het SETTINGS gedeelte 'interface' verwijst naar de netwerkpoort waarop de omvormer is aangesloten. Als je sectie 4.1 hebt gevolgd is het waarschijnlijk 'enp4s0'.

Zet het bestand 'se-logger.service' in de '/etc/systemd/system' directory. Draai je Raspbian? Dan moet je de 'KillMode=' regel aanpassen naar 'KillMode=control-group'. Als je sectie 4.2 had gevolgd (logger via WiFi verbonden met het internet) is het aan te raden om /etc/systemd/system/se-nat.service aan te passen en de 'After=' regel te veranderen naar 'After=network.target se-logger.service'. Dit zorgt ervoor dat de omvormer pas toegang krijgt tot internet als de logger service draait.

```bash
sudo systemctl enable se-logger.service
sudo systemctl start se-logger.service
```

SolarEdge Optimizer serial numbers:

|  Serial# | Panel# |
|----------|--------|
| 1159335C-F9 | 1.1.1 |
| 11593696-36 | 1.1.2 |
| 1159344B-E9 | 1.1.3 |
| 11593431-CF | 1.1.4 |
| 11592B4D-E2 | 1.1.5 |
| 115927E1-72 | 1.1.6 |
| 20506388-5B | 1.1.7 |
| 11592565-F4 | 1.1.8 |
| 11592569-F8 | 1.1.9 |
| 1159255F-EE | 1.1.10 |
| 2050636F-42 | 1.1.11 |
| 11592F61-FA | 1.1.12 |
| 1159279A-2B | 1.1.13 |
| 11592E30-C8 | 1.1.14 |
| 20506344-17 | 1.1.15 |
| 11592F5F-F8 | 1.1.16 |
| 1159299D-30 | 1.1.17 |
| 1159299B-2E | 1.1.18 |
| 1159328F-2B | 1.1.19 |
| 11592836-C8 | 1.1.20 |
| 11593144-DF | 1.1.21 |
| 11593457-F5 | 1.1.22 |
| 11593586-25 | 1.1.23 |
| 205061ED-BE | 1.1.24 |
