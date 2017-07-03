
### Cyber Security GeoIP Attack Map Visualization
This geoip attack map visualizer was developed to display network attacks on your organization in real time. The data server follows a syslog file, and parses out source IP, destination IP, source port, and destination port. Protocols are determined via common ports, and the visualizations vary in color based on protocol type. [CLICK HERE](https://www.youtube.com/watch?v=zTvLJjTzJnU) for a demo video. This project would not be possible if it weren't for Sam Cappella, who created a cyber defense competition network traffic visualizer for the 2015 Palmetto Cyber Defense Competition. I mainly used his code as a reference, but I did borrow a few functions while creating the display server, and visual aspects of the webapp. I would also like to give special thanks to [Dylan Madisetti](http://www.dylanmadisetti.com/) as well for giving me advice about certain aspects of my implementation.

### Important
This program relies entirely on syslog, and because all appliances format logs differently, you will need to customize the log parsing function(s). If your organization uses a security information and event management system (SIEM), it can probably normalize logs to save you a ton of time writing regex.
1. Send all syslog to SIEM.
2. Use SIEM to normalize logs.
3. Send normalized logs to the box (any Linux machine running syslog-ng will work) running this software so the data server can parse them.
4. For ease of use, we have parsed the logs to the following format. 

            -Source_IP, Destination_IP, Source_port, Destination_port

### Configs 
1. Make sure in **/etc/redis/redis.conf** to change **bind 127.0.0.1** to **bind 0.0.0.0** if you plan on running the DataServer on a different machine than the AttackMapServer.
2. Make sure **syslog_path** is set to the file receiving formatted syslog
3. Make sure **db_path** is set to the location hosting the **GeoLite2-City.mmdb**
4. Make sure **hq_ip** is set 
5. Make sure that the WebSocket address in **/AttackMapServer/index.html** points back to the IP address of the **AttackMapServer** so the browser knows the address of the WebSocket.
6. Make sure **Static** & **Flags** is set to location of unzipped files
7. Download & Unzip the MaxMind GeoLite2 database, and change the **db_path** variable in **DataServer.py** to wherever you store the database.
    * ./db-dl.sh
7. Add headquarters latitude/longitude to hqLatLng variable in **index.html**
8. Add API token for mapbox to **L.Mapbox.accessToken** in **map.js**
9. Set **map = L.mapbox.map("map", "mapbox.dark", {** This allows for a darkened satellite map of the world 
10. Set HQ (College Station) Coordinates via **var  hqLatLng = new L.LatLng(30.6280, -96.3344);**
11. Use syslog-gen.py, or syslog-gen.sh to simulate dummy traffic "out of the box."
12. **IMPORTANT: Remember, this code will only run correctly in a production environment after personalizing the parsing functions. The default parsing function is only written to parse ./syslog-gen.sh traffic.**

### Bugs, Feedback, and Questions
If you find any errors or bugs, please let me know. Questions and feedback are also welcome and can be documented in this repository.
