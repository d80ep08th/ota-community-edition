# OTA-Community-Edition

- The OTA Community Edition is open-source server software to allow over-the-air (OTA) updates of compatible clients

- It is comprised of a number of services which together make up the OTA system.

- The source code for the servers is available on [Github](https://github.com/advancedtelematic) and is licensed under the MPL2.0

- Docker container images of the latest build are available on [Docker Hub](https://hub.docker.com/u/advancedtelematic).

- See the [Aktualizr](https://github.com/advancedtelematic/aktualizr) open-source example client
---
1. Run the script **gen-server-certs.sh**
- Generate the required certificates.
- This is required to provision the aktualizr.
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

3. Pull from docker (or build docker image)

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
> Clean everything
```
docker compose -f ota-ce.yaml rm
docker volume ls | grep ota-lith | awk '{print $2}'| xargs -n1 docker volume rm

```
5. Test if the servers are up

```
curl director.ota.ce/health/version
```

6. Run the script **get-credentials.sh**
> Creates credentials.zip in `ota-ce-gen/` directory
 ```
 ./scripts/get-credentials.sh
 ```

7. Run the script **gen-device.sh**
> It provides a new device, by creating a new directory `**ota-ce-gen/devices/:uuid**` where **uuid** is the id of the new device.

 ```
 ./scripts/gen-device.sh
 ```
8. Run **aktualizr** :
- In the directory `**ota-ce-gen/devices/:uuid**`
> To deploy updates to the devices make we connect aktualizr
```
aktualizr --run-mode=once --config=config.toml
```   

---
## License

This code is licensed under the [Mozilla Public License 2.0](LICENSE), a copy of which can be found in this repository. All code is copyright [ATS Advanced Telematic Systems GmbH](https://www.advancedtelematic.com), 2016-2018.
