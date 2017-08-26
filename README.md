# Dex BOSH Release

This is a [BOSH](http://bosh.io/) release for [Dex](https://github.com/coreos/dex).

## Usage

### Requirements

In order to use this BOSH release you will need:

* [BOSH CLI v2](https://bosh.io/docs/cli-v2.html)
* An already deployed [BOSH environment](http://bosh.io/docs/init.html)
* A compatible [cloud-config](http://bosh.io/docs/terminology.html#cloud-config) with a `default` option for `network` and `vm_types` (you can use the example that comes from [cf-deployment](https://github.com/cloudfoundry/cf-deployment/blob/master/bosh-lite/cloud-config.yml))

###  Clone the repository

First, clone this repository into your workspace:

```
git clone https://github.com/frodenas/dex-boshrelease
cd dex-boshrelease
export BOSH_ENVIRONMENT=<name>
```

### Basic deployment

To deploy a basic `dex` server use the following command:

```
bosh -d dex deploy manifests/dex.yml \
  --vars-store tmp/deployment-vars.yml
```

### Operations files

Additional [operations files](http://bosh.io/docs/cli-ops-files.html) are located at the [manifests/operators](https://github.com/frodenas/dex-boshrelease/tree/master/manifests/operators) directory. Those files includes a basic configuration, so extra ops files might be needed for additional configuration.

Please review the op files before deploying them to check the requeriments, dependencies and necessary variables.

| File | Description |
| ---- | ----------- |
| [add_static_ips.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/add_static_ips.yml) | Adds a static IP to dex |
| [connector_github.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/connector_github.yml) | Adds a [Github](https://github.com/coreos/dex/blob/master/Documentation/github-connector.md) connector |
| [connector_gitlab.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/connector_gitlab.yml) | Adds a [Gitlab](https://github.com/coreos/dex/blob/master/Documentation/gitlab-connector.md) connector |
| [connector_oidc.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/connector_oidc.yml) | Adds an [OpenID Connect](https://github.com/coreos/dex/blob/master/Documentation/oidc-connector.md) connector |
| [connector_uaa.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/connector_uaa.yml) | Adds a [Cloud Foundry UAA](https://docs.cloudfoundry.org/concepts/architecture/uaa.html) connector |
| [storage_kubernetes.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/storage_kubernetes.yml) | Uses a Kubernetes ThirdPartyResource to persist state |
| [storage_postgres.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/storage_postgres.yml) | Uses a PostgreSQL database to persist state |
| [storage_sqlite3.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/storage_sqlite3.yml) | Uses a SQLite3 database to persist state |
| [use_ssl.yml](https://github.com/frodenas/dex-boshrelease/blob/master/manifests/operators/use_ssl.yml) | Enables dex TLS (web & grpc) |

### Deployment variables and the var-store

Some operators files requires additional information to provide environment-specific or sensitive configuration such as various credentials. To do this in the default configuration, we use the `--vars-store`. This flag takes the name of a `yml` file that it will read and write to. Where necessary credential values are not present, it will generate new values based on the type information stored at the different deployment files. Necessary variables that BOSH can't generate need to be supplied as well.
See each particular op files you're using for any additional necessary variables.

See also the [BOSH CLI documentation](http://bosh.io/docs/cli-int.html#value-sources) for more information about ways to supply such additional variables.

## Contributing

Refer to [CONTRIBUTING.md](https://github.com/frodenas/dex-boshrelease/blob/master/CONTRIBUTING.md).

## License

Apache License 2.0, see [LICENSE](https://github.com/frodenas/dex-boshrelease/blob/master/LICENSE).
