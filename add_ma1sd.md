***
## Add ma1sd identity server with synapse identity store
### Install
We will install the Debian package as easy way to get it up and running. As there isn'T a dedicated repo for ma1sd, we need to download the latest release:
* Download the latest release debian package under https://github.com/ma1uta/ma1sd/releases/latest
* Ma1sd needs a working Java install, choose one: 
  * sudo apt install openjdk-11-jdk-headless
  * sudo apt install openjdk-8-jdk-headless
* Install package: sudo dpkg -i ma1sd_2.4.0_all.deb 
### Config
#### Nginx
Ma1sd needs to be introduced to the API calls that take care of identity. The identiy location block must be placed before the default matrix locations blocks 
```
location /_matrix/identity {
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_pass http://127.0.0.1:8090/_matrix/identity;
}
```
#### Synapse
Add your ma1sd domain into the homeserver.yaml at trusted_third_party_id_servers and restart synapse.
In a typical configuration, you would end up with something similar to:
```
trusted_third_party_id_servers:
    - matrix.example.org
```
It is highly recommended to remove matrix.org and vector.im (or any other default entry) from your configuration so only your own Identity server is authoritative for your HS.

#### Ma1sd
To activate the synapse identity store, place following snippet intoma1sd.yaml:
```
###################
# Identity Stores #
###################
# If you are using synapse standalone and do not have an Identity store,
# see https://github.com/ma1uta/ma1sd/blob/master/docs/stores/synapse.md#synapse-identity-store
#
# If you would like to integrate with your AD/Samba/LDAP server,
# see https://github.com/ma1uta/ma1sd/blob/master/docs/stores/ldap.md
#
# For any other Identity store, or to simply discover them,
# see https://github.com/ma1uta/ma1sd/blob/master/docs/stores/README.md
synapseSql:
  enabled: true
  type: postgresql
  connection: //127.0.0.1:5432/synapsedb?user=synapseuser&password=password
```
