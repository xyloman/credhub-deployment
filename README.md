# credhub-deployment

> A toolchain for deploying credhub with [BOSH](https://bosh.io).

This repository is a community-maintained set of [manifests](http://bosh.io/docs/manifest-v2.html) and [opsfiles](http://bosh.io/docs/cli-ops-files.html) useful for deploying Credhub in various configurations to various IaaSes.

* Leverages [credhub-release](https://github.com/pivotal-cf/credhub-release)

## Example Usage

### Bosh Deploy

```bash
bosh deploy ./credhub.yml \
  --vars-store ./credhub-creds.yml \
  --vars-file ./credhub-vars.yml \
  -o ./operations/target-group.yml \
  -o ./operations/scale.yml \
  --var-file postgres_ca_cert=./rds-ca-2015-root.pem \
  --var-file uaa_jwt_public_key=./uaa_jwt_public_key.key \
  --var-file uaa_tls_ca=./acm-ca.pem
```

### Example credhub-vars.yml

```yaml
# release
credhub_version: '1.6.5'
credhub_sha1: 'eda4e8873aa2dbfacb1857b175f761d2d0b64538'

# target-group.yml for external load balancer
credhub_target_group_name: 'credhub-target-group'

# scale.yml
credhub_instances: 2
credhub_azs: [z1, z2, z3]

# credhub release
credhub_vm_type: 'r4.large'
credhub_host: "credhub.example.com"
uaa_host: "uaa.example.com"
network_name: "default"

# credhub postgres database
credhub_database_host: "hostname"
credhub_database_port: "5432"
credhub_database_username: "credhub"
credhub_database_password: "password"
```

## Operator Files

### scale.yml

credhub_azs - Update a credhub deployment to scale accross multiple AZs.
credhub_instances - Update a credhub deployment to change the number of instances of the service that is ran (e.g. VM Instances).

### target-group.yml

credhub_target_group_name - Update a credhub deployment to associate the credhub instance group with a vm_extension target_group used in AWS to associate an external load balancer or target group configured in cloud config.