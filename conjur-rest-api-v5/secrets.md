
### Add a secret

If we wants to secure a database password using CyberArk, where the value is `c3c60d3f266074` in `db\password` variable, we can execute the following command:

```
source showSettings.sh && \
curl -s -H "Authorization: Token token=\"${access_token}\"" \
     -X POST --data "c3c60d3f266074" \
     http://172.30.1.2:8080/secrets/default/variable/db%2Fpassword 
```{{execute}}

And the value is secured by Conjur.

### Retrieve a secret

To get the secret, execute the following command

```
source showSettings.sh && \
curl -s -H "Authorization: Token token=\"${access_token}\"" \
     http://172.30.1.2:8080/secrets/default/variable/db%2Fpassword 
```{{execute}}

