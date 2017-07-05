Tested on CentOS 7 with SELinux enforcing. 

* Install Git & Wget
  ```sh
  sudo yum install git wget
  ```

* Install EPEL Repository & update YUM
  ```sh
  sudo yum install epel-release
  sudo yum update
  ```

* Install Redis 
  ```sh
  sudo yum install redis
  ```

* Start Redis 
  ```sh
  sudo systemctl start redis
  ```

* Automatically start Redis on boot 
  ```sh 
  sudo systemctl enable redis
  ```

* Verify Redis is running (should respond with **Pong**)
  ```sh 
  redis-cli ping 
  ```  
  
* Install Pip3
  ```sh
  sudo yum install python34-setuptools
  sudo easy_install-3.4 pip
  ```

* Create folder inside **/opt**
  ```sh
  mkdir /opt/MissileMap
  ```
  
* Clone the application:

  ```sh
  git clone https://github.com/matthewclarkmay/geoip-attack-map.git
  ```

* Install python requirements:

  ```sh
  cd /var/MissileMap/geoip-attack-map
  sudo pip3 install -U -r requirements.txt

  ```
  
* Start Redis Server:

  ```sh
  redis-server

  ```
* Configure the Data Server DB:
  
    ```sh
  cd DataServerDB
  ./db-dl.sh
  cd ..

  ```
* Start the Data Server:

    ```sh
  cd DataServer
  sudo python3 DataServer.py

  ```
  
* Start the Syslog Gen Script, inside DataServer directory:

  * Open a new terminal tab (Ctrl+Shift+T, on Ubuntu).
  
    ```sh
    ./syslog-gen.py
    ./syslog-gen.sh
    ```

* Configure the Attack Map Server, extract the flags to the right place:

  * Open a new terminal tab (Ctrl+Shift+T, on Ubuntu).
  
    ```sh
    cd AttackMapServer/
    unzip static/flags.zip
    ``` 
 
* Start the Attack Map Server:
  
    ```sh
    sudo python3 AttackMapServer.py
    ```
 
* Access the Attack Map Web Interface:
      
    * To access via browser on another computer, use the external IP of the machine running the AttackMapServer.
    
     * Edit the IP Address in the file "/static/map.js" at "AttackMapServer" directory. From:
      
       ```javascript
       var webSock = new WebSocket("ws:/127.0.0.1:8888/websocket");
       ```
     * To, for example: 
     
       ```javascript
       var webSock = new WebSocket("ws:/IP-ADDRESS:8888/websocket");
       ```
     * Restart the Attack Map Server:
     
       ```sh
       sudo python3 AttackMapServer.py
       ```
     * On the other computer, points the browser to:
     
       ```sh
       http://IP-ADDRESS:8888/
       ```
