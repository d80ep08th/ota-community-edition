# OTA-Community-Edition

- The OTA Community Edition is open-source server software to allow over-the-air (OTA) updates of compatible clients

- It is comprised of a number of services which together make up the OTA system.

- The source code for the servers is available on [Github](https://github.com/advancedtelematic) and is licensed under the MPL2.0

- Docker container images of the latest build are available on [Docker Hub](https://hub.docker.com/u/advancedtelematic).

- See the [Aktualizr](https://github.com/advancedtelematic/aktualizr) open-source example client
---
1. Generate the required certificates using the script **gen-server-certs.sh**
>This is required to provision the aktualizr
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

|     Options     |Pull from Docker            | Build Docker Image            |
|----------------|-------------------------------|-----------------------------|
|Number of Steps|3 steps|1 step only|
|I|`export img=uptane/ota-lith:$(git rev-parse master)`|`sbt docker:publishLocal`|
|II|`docker pull $img`|      ~     |
|III|`docker tag $img uptane/ota-lith:latest`|~|

4. Run docker-compose


> Start OTA-CE
```
docker compose -f ota-ce.yaml up
```
> Stop OTA-CE
```
docker compose -f ota-ce.yaml down
```
> Make sure everything is clean
```
docker compose -f ota-ce.yaml rm
docker volume ls | grep ota-lith | awk '{print $2}'| xargs -n1 docker volume rm

```
5. Test

```
curl director.ota.ce/health/version
```

6. Get credentials using the script **get-credentials.sh**

 ```
 ./scripts/get-credentials.sh
 ```

7. You can now create device credentials and provision devices using the script **gen-device.sh**

 ```
 ./scripts/gen-device.sh
 ```
 - This will create a new dir in **ota-ce-gen/devices/:uuid** where **uuid** is the id of the new device.

8. Run **aktualizr** in that directory using:

```
aktualizr --run-mode=once --config=config.toml
```   
  - You can now deploy updates to the devices
---
## License

This code is licensed under the [Mozilla Public License 2.0](LICENSE), a copy of which can be found in this repository. All code is copyright [ATS Advanced Telematic Systems GmbH](https://www.advancedtelematic.com), 2016-2018.
