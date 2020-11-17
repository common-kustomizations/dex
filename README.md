# Dex
*A Federated OpenID Connect Provider*

## Resources
* [Homepage](https://dexidp.io/)
* [Documentation](https://dexidp.io/docs/)
* [Example Dev Configuration](https://github.com/dexidp/dex/blob/master/examples/config-dev.yaml)

## kustomization
This kustomization will create the following Kubernetes Resources
* A [Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) *dex* in which the application resides
* A [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) + [Service](https://kubernetes.io/docs/concepts/services-networking/service/) *dex-server* for the main server backend.
* An example [ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/) + [Secret](https://kubernetes.io/docs/concepts/configuration/secret/) *dex-server* for configuration of the main server backend.

## Configuration
The application can be configured by overwriting the ConfigMap *dex-server*s key *config.yml* with a valid Dex
configuration file. 
This file can have references to environment variables via `$KEY` syntax.
In addition the Secret *dex-server* serves as a provider for the servers environment.
