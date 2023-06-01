# monitor_service_led

<div style="text-align: center">
<img src="schema.png"
     alt="schema"/>
</div>


Simple script to display service or Docker container status on WAGO Controller user LEDs.
- GREEN : Container or service running
- ORANGE : Container or service stopped 
- RED : Container does not exist or Docker not installed/started (not used for service)

### Installation
Download and place the script monitor_container_led i.e. folder /etc/config-tools.

Give it execution permission :
```shell
chmod 750 /etc/config-tools/monitor_service_led
```


### Usage

```shell
./monitor_service_led -s [SERVICE or CONTAINER NAME] -l [LED ID] (-d)
```

-h : display this help
-s : service name or container name if -d option is used
-l : Led ID where service status should be displayed. Acceptable values : U1 - U7 depending on the platform
-d : argument to provide if the service to monitor is a Docker container

Example 1 : monitor lighttpd web server status : 


```shell
./monitor_service_led -s lighttpd -l U1
```
Example 2 : monitor mosquitto container status : 

```shell
./monitor_service_led -s mosquitto -l U1 -d
```

Make sure LED isn't used by CODESYS and is available on the PFC variant. 

### Set periodic check

Then you need to use CRON in order to trigger a period call of the script. 

To open crontab with nano (easier than vim ?)
```shell
export VISUAL=nano; crontab -e
```
Copy/Paste the following (adapt to your script location, container/service name and LED) :
```shell
* * * * *  /etc/config-tools/monitor_service_led -s mosquitto -l U1 -d
```
This will check the container state every minute. 
You can add several lines if you have several containers to monitor

### Go further
We could add a deeper check of the container based on logs or based on status (when provided by the application, i.e Node-RED).
When using a device with only 1 User LED (750-8217, CC100...) we could adapt the script to display a synthesis of monitored containers.
Find a way to blink LED could be useful too.

