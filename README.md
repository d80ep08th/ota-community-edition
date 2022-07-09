# OTA-Community-Edition

> If you don't have kafka or mariadb running and just want to try ota-ce

> Run using docker-compose:
### Running With Docker Compose
1. Generate the required certificates using the script `gen-server-certs.sh`
```
./scripts/gen-server-certs.sh
```
2. Update /etc/hosts with the following host names:

```
0.0.0.0         reposerver.ota.ce
0.0.0.0         keyserver.ota.ce
0.0.0.0         director.ota.ce
0.0.0.0         treehub.ota.ce
0.0.0.0         deviceregistry.ota.ce
0.0.0.0         campaigner.ota.ce
0.0.0.0         app.ota.ce
0.0.0.0         ota.ce
```

3. Build Docker Image or Pull from Docker

|      Steps     |Pull from Docker            | Build Docker Image            |
|----------------|-------------------------------|-----------------------------|
|I|`export img=uptane/ota-lith:$(git rev-parse master)`|`sbt docker:publishLocal`|
|II|`docker pull $img`|      ~     |
|III|`docker tag $img uptane/ota-lith:latest`|~|

4. Run docker-compose
``` 
docker-compose -f ota-ce.yaml up
```

5. Test

For example 
```
curl director.ota.ce/health/version
```

6. You can now create device credentials and provision devices using the script `gen-device.sh`

 ```
 ./scripts/gen-device.sh
 ```
 > This will create a new dir in `ota-ce-gen/devices/:uuid` where `uuid` is the id of the new device. You can 

7. Run `aktualizr` in that directory using:
```
aktualizr --run-mode=once --config=config.toml
```   
  > You can now deploy updates to the devices
---
---

