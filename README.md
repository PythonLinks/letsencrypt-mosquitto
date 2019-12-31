# LetsEncrypt Mosquitto Docker Container  

This Docker Container makes it really easy to create encrypted connections to an MQTT server.  It uses lets encrypt to generate certificates which the Mosquitto MQTT broker then uses.

This docker container includes both the Caddy web server and the Mosquitto MQTT broker.  The caddy web server generates the ssl certificates, which the
mosquitto broker then uses. 

The idea is to make life as easy as possible for you, the user. Here is the command to run the container.  

`
docker run --name mosquitto -it \
    -e DOMAIN=mosquitto.cloud-native.pl \
    -e EMAIL=me@domain.com \
    -e UID=$UID \
    -v /path/to/my/root/:/root \
    -v /root/s6/root/mosquitto.conf:/mosquitto/mosquitto.conf \
    -p 80:80 \
    -p 443:443 \
    -p 8080:8080 \
    -p 8081:8081 \
    -p 1883:1883 \
    -p 8883:8883 \
     pythonlinks/letsencrypt-mosquitto:test

#    IF YOU WANT TO SERVE YOUR OWN WEB FILES
#    -v /root/s6/www:/var/www \

#    AND HERE IS DOCUMENTATION OF THE VARIOUS PORTS
#    -p 80:80       # HTTP  serve files from /var/www
#    -p 443:443     # HTTPS serve files from /var/www
#    -p 8080:8080   # WebSockets No Encryption 
#    -p 8081:8081   # WebSockets Encrypted
#    -p 1883:1883   # MQTT No Encryption 
#    -p 8883:8883   # MQTT Encrypted


`
Inside the docker container there is one defined user, the operator.  
The operator has the same user id as the account which starts the container. 





There is always an issue with OS and docker permissions conflicting.
Traditionally web is served from group 

