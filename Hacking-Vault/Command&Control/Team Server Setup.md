
The team server will run [[Cobalt Strike]] software. 

## 🖊️ Basic Execution

After installing all components, you can run it using:

attacker@ubuntu ~/cobaltstrike> sudo ./teamserver "IP or Domain of Team Server" "Password" "location/to/malleableprofile"


## 📔 Run as a service

Running the team server as a service allows it to start automatically when the VM starts up, which obviously saves us having to SSH in each time and start it manually.

1. First, create the file in `/etc/systemd/system`.

`attacker@ubuntu ~> sudo vim /etc/systemd/system/teamserver.service

2. Then paste the following content:

```
[Unit]
Description=Cobalt Strike Team Server
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=root
WorkingDirectory=/home/attacker/cobaltstrike
ExecStart=/home/attacker/cobaltstrike/teamserver 10.10.5.50 Passw0rd! c2-profiles/normal/webbug.profile

[Install]
WantedBy=multi-user.target

```

3. Next, reload the systemd manager and check the status of the service.  It will be inactive/dead.

```
attacker@ubuntu ~> sudo systemctl daemon-reload
attacker@ubuntu ~> sudo systemctl status teamserver.service
● teamserver.service - Cobalt Strike Team Server
     Loaded: loaded (/etc/systemd/system/teamserver.service; disabled; vendor preset: enabled)
     Active: inactive (dead)
```

4. Start the service and check its status again.

```
attacker@ubuntu ~> sudo systemctl start 
teamserver.service
attacker@ubuntu ~> sudo systemctl status 
teamserver.service
● teamserver.service - Cobalt Strike Team Server
     Loaded: loaded (/etc/systemd/system/teamserver.service; disabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-09-05 08:25:26 UTC; 14s ago

```

5. The service should be active/running and you will see the typical console output from the team server.  Now that we know the service is working, we can tell it to start on boot.

```
attacker@ubuntu ~> sudo systemctl enable teamserver.service

Created symlink /etc/systemd/system/multi-user.target.wants/teamserver.service → /etc/systemd/system/teamserver.service.
```

  We will now be able to connect from the Cobalt Strike client as soon as the VMs boot up.

### Properties
---
📆 created   {{06-02-2024}} 15:11
🏷️ tags: #redteam #crto   
---

