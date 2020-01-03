# Lets Encrypt Mosquitto Docker Container  

[This Docker Container](https://hub.docker.com/r/pythonlinks/letsencrypt-mosquitto) makes it really easy to create encrypted connections to an MQTT server.  It uses the caddy web server to generate lets encrypt certificates which the Mosquitto MQTT broker then uses.  You can also serve files using the include [caddy web server](https://caddyserver.com/v1/).  The idea is to make life as easy as possible for the user. 

## Starting the Broker
First you need a server on the web.  I recommend Linode.com.  Choose their $5.00/month Ubuntu server.  Follow their instructions for logging in.  Then [install docker on Ubuntu](https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04).  

Once Docker is installed, here is the command to run the container.  

```
docker run --name mosquitto -it \
    -e DOMAIN=www.your-domain.com \
    -e EMAIL=your-email-address@your-domain.com \
    -e UID=$UID \
    -v letsencrypt:/root/.caddy \
    -v caddy:/root/.caddy \
    -v mosquitto:/mosquitto \
    -p 80:80 \
    -p 443:443 \
    -p 8080:8080 \
    -p 8081:8081 \
    -p 1883:1883 \
    -p 8883:8883 \
     pythonlinks/letsencrypt-mosquitto:latest

```
 ### Where 
   *docker run* runs the container 
   <br>
   *-it* runs it in the foreground.  That way you can watch what is happeneing. 
   To run it in the background, use -*d*.  
   *letsencrypt* is where the lets encrypt certificates and keys are stored. 
   <br>
   *caddy* is where caddy stores its config file and certificates
   <br>
   *mosquitto* is where the mosquitto configuration files are stored.
```
You can then see where to find and edit the data volumes using 
      docker volume inspect caddy
      docker volume inspect mosquitto

IF YOU WANT TO SERVE YOUR OWN WEB FILES ADD THE FOLLOWING LINE
    -v /path/to/your/website/:/var/www\/ \

AND HERE IS DOCUMENTATION OF THE VARIOUS PORTS
    -p 80:80       # HTTP  serve files from /var/www/
    -p 443:443     # HTTPS serve files from /var/www/
    -p 8080:8080   # WebSockets No Encryption 
    -p 8081:8081   # WebSockets Encrypted
    -p 1883:1883   # MQTT No Encryption 
    -p 8883:8883   # MQTT Encrypted/

```
## Permissions
Inside the docker container there is one defined user, the operator.  His group is www-data.
The operator has the same user id as the account which starts the container.  That way 
you and the servers can both read and write to the configuration and web server files. 

## Mosquitto Configuration. 
The mosquitto configuration file is located inside of the docker container at 
```
/mosquitto/mosquitto.conf.  
```
DO NOT EDIT MOSQUITTO.CONF DIRECTLY. 
Users may change their domain name, so
mosquitto.conf gets overwritten on every startup
after a certificate is available. 
Instead what you want to do is edit the file
```
/mosquitto/mosquitto.conf.template
```
and then restart the docker container to run the template.

You probably want to edit the template from the host computer your favorite editor.
But  first you have to find the file. To find where the
docker stores the template data type:
```
docker volume inspect mosquitto
```

## Questions
<p> Please <a href="mailto:lozinski@PythonLinks.info">send me an email</a>
if you have any questions.  You can also raise issues or edit the documentation on the [github page](https://github.com/PythonLinks/letsencrypt-mosquitto/blob/master/README.md)
</p>    

