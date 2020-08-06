# EdgeX Foundry on KubeEdge

A [Helm](https://helm.sh/) chart to easily deploy the EdgeX IoT project on KubeEdge.
Based on EdgeX [Geneva](https://github.com/edgexfoundry/developer-scripts/tree/master/releases/geneva/compose-files) version.

## Prerequisites  
- Kubernetes master 1.10+
- KubeEdge 1.3.0+
- [Helm](https://helm.sh/) 2.8.0+

## Installation

Install the EdgeX helm chart with a release name edgex-geneva

- helm2

```bash
$ git clone https://github.com/DaveZLB/edgex-helm.git
$ helm install --name edgex-geneva edgex-helm
```

- helm3

```bash
$ git clone https://github.com/DaveZLB/edgex-helm.git
$ helm install edgex-geneva edgex-helm
```

## Uninstallation

- helm2

```bash
helm delete edgex-geneva --purge
```

- helm3

```bash
helm uninstall edgex-geneva
```

## Test EdgeX

1.On the edge node of Kubeedge:  
```bash
curl http://<service_name>.<service_namespace>.svc.<cluster>.<local>:<port>/api/v1/ping  
```  
2.At any point in the same subnet as the edge node  
```bash
curl http://<ip>:<port>/api/v1/ping
```  

## Tips
- The UI currently has some problems accessing the Kubeedge service, so you need to manually change the env parameter in the YAML file to the IP address of the service's POD.  
- Since Kubeedge's Edgemesh only supports forwarding of HTTP protocol, other services cannot connect with redis via svc, and need to manually add the IP of the node where Redis POD is located to connect.[specific explanation](https://github.com/kubeedge/kubeedge/issues/1837)
- This project is based on [docker-compose-geneva-redis-no-secty.yml](https://github.com/edgexfoundry/developer-scripts/blob/master/releases/geneva/compose-files/docker-compose-geneva-redis-no-secty.yml),
you can implement your customized version based on this.
- Since other edgex services need to rely on consul to obtain configuration or register themselves to consul, other services cannot run normally until consul starts successfully.
- Unlike the docker-compose files for this release (which use a separate Docker volume container), the manifest files mount host based volumes as follows:

	1、edgex-core-consul's /consul/config directory is mapped to the host's /mnt/edgex-consul-config directory.
	
	2、edgex-core-consul's /consul/data directory is mapped to the host's /mnt/edgex-consul-data directory.
	
	3、edgex-db's /data/db directory is mapped to the host's /mnt/edgex-db directory.
	
	4、edgex-support-logging's /edgex/logs directory is mapped to the host's /mnt/edgex-support-logging directory.


## Some articles about deploying edgex to kubernetes

- VMware China R&D Center
https://mp.weixin.qq.com/s/ECdEkc9QdkVScn4Lvl_JJA

## Feedback

If you find a bug or want to request a new feature, please open a [GitHub Issue](https://github.com/DaveZLB/edgex-helm/issues)


