In this step, we will setup a Conjur OSS container, load some policies and create a machine identity for the host

### Install Conjur
Let's pull and setup a Conjur OSS.   It will take a couple of moments
`./setupConjur.sh https://[[HOST_SUBDOMAIN]]-443-[[KATACODA_HOST]].environments.katacoda.com/conjur`{{execute}}

### Prepare Conjur Policy
The sample policies have been prepared for you. 

**Root policy**

Run `cat conjur.yml`{{execute}} to review the root policy
```
- !policy
  id: cert

- !policy
  id: envoy
```
**cert policy**

Run `cat cert.yml`{{execute}} to review the root policy

```
- &variables
  - !variable private_key
  - !variable cert_chain

- !group secrets-users

- !permit
  resource: *variables
  privileges: [ read, execute ]
  roles: !group secrets-users

- !grant
  role: !group secrets-users
  member: !layer /envoy
```

**envoy policy**

Run `cat envoy.yml`{{execute}} to review the root policy

```
- !layer

- !host envoy-01

- !grant
  role: !layer
  member: !host envoy-01
```
### Load Conjur Policies

Now let's copy the policy files to Conjur CLI container and load them

**Load Root Policy**

```
docker cp conjur.yml root_client_1:/tmp/
docker-compose exec client conjur policy load --replace root /tmp/conjur.yml
```{{execute}}

**Load Envoy Policy**
```
docker cp envoy.yml root_client_1:/tmp/
docker-compose exec client conjur policy load envoy /tmp/envoy.yml | tee frontend.out
```{{execute}}

**Load cert Policy**
```
docker cp cert.yml root_client_1:/tmp/
docker-compose exec client conjur policy load krb5 /tmp/cert.yml
```{{execute}}

### Add certificate chain & private key as variables
Copy the `cyberarkdemo-com.crt` & `cyberarkdemo-com.key` files to Conjur CLI container and add to Conjur as a variable

```
docker cp cyberarkdemo-com.crt root_client_1:/tmp/
docker cp cyberarkdemo-com.key root_client_1:/tmp/

docker-compose exec client bash -c "head -c1024 /tmp/cyberarkdemo-com.crt | conjur variable values add cert/cert_chain"
docker-compose exec client bash -c "head -c1024 /tmp/cyberarkdemo-com.key | conjur variable values add cert/private_key"
```{{execute}}

### Cleanup 
It's time to remove the keytab files

```
rm cyberarkdemo-com.crt
rm cyberarkdemo-com.key
docker-compose exec client bash -c "rm /tmp/cyberarkdemo-com.crt"
docker-compose exec client bash -c "rm /tmp/cyberarkdemo-com.key"
```{{execute}}

To verify:
`ls cyberarkdemo-com.*`{{execute}}

`docker-compose exec client bash -c "ls /tmp/cyberarkdemo-com.*"`{{execute}}