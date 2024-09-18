# Generic helm chart for application

Repository contains generic helm chart for deploying any application, based on the official Helm chart for Kubernetes, but with some useful modifications.

Such as:

* [`Nordlynx`](https://support.nordvpn.com/hc/en-us/articles/19564565879441-What-is-NordLynx)  VPN container as a sidecar.
* Ability to define PersistentVolumeClaim for the application.
* Ability to define environment variables for the application  as a secret.
* Multiple port definitions for the service.

For more information check [`charts/application/values.yaml`](charts/application/values.yaml) file.

## Configuration

### Nordlynx VPN

 ```yaml
nordlynx:
  enabled: false
  privateKey: # nordlynx private key
  netLocal: # CIDR networks (IE 192.168.1.0/24), add a route to allows replies once the VPN is up.
            # example: 10.42.0.0/16 10.43.0.0/16
  dns: # A comma-separated list of IP (v4 or v6) addresses to be set as the interface's DNS servers, or non-IP hostnames to be set as the interface's DNS search domains.
       # example: 10.43.0.10,192.168.1.1
  query: # Query for the api nordvpn
         # example: filters\\[country_id\]=225
 ```

### PersistentVolumeClaim

 ```yaml
persistence:
  enabled: false
  accessMode: ReadWriteOnce
  size: 1Gi
 ```

### Environment variables as a secret

 ```yaml
secretEnv:
  VAR_NAME: variable value
 ```

### Multiple port definitions

 ```yaml
connectivity:
  hostNetwork: false
  service:
    type: ClusterIP
  ports:
    - name: http
      protocol: TCP
      containerPort: 80
      servicePort: 80
      nodePort: 
  probes:
    readiness:
      port: http
  annotations: {}
 ```

## Usage

How to use this helm chart you can find in the [`helm-charts`](https://github.com/kostiantyn-matsebora/helm-charts) repository.
