
### Web Access

Once ready, you should be able to access various systems on this environment by accessing the following links:

- External: [Conjur (External access to 8080)]({{TRAFFIC_HOST1_8080}})
- Internal: https://proxy:8443

### Conjur CLI

Conjur CLI has been installed for you.
To check the version installed, execute:
```
conjur --version
```{{execute}}


### Conjur Init

You only to initialize Conjur CLI once.

```
conjur init  -a default -u https://proxy:8443 --self-signed
```{{execute}}

If `Trust this certificate? yes/no (Default: no):` is prompted, answer:
 ```
 yes
 ```{{execute}}

If the above command returns an error message, that means the system is being started.
Please wait for a moment and try again.


### Login to Conjur

Let's login to Conjur, and admin password of the pre-configured environment is `b81t11ebd2en115rjc3bbyfhhhtvcttyc0bm42jcagzreb8pd7`
```
conjur login -i admin -p b81t11ebd2en115rjc3bbyfhhhtvcttyc0bm42jcagzreb8pd7
```{{execute}}

### Bonus: Jenkins

Link to Jenkins: {{TRAFFIC_HOST1_8081}}

```
The page doesn't load successfully? 
No worries, it's being started.  
Please click the above link again in 1 or 2 minutes
```

Password to unlock Jenkins
```
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```{{execute}}

### Have fun!

Check out the [Conjur doc](https://docs.cyberark.com/Product-Doc/OnlineHelp/AAM-DAP/Latest/en/Content/Developer/CLI/cli-commands.htm?tocpath=Developer%7CConjur%20CLI%7CConjur%20CLI%20Command%20Reference%7C_____0) for more details
