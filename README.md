[![Sensu Bonsai Asset](https://img.shields.io/badge/Bonsai-Download%20Me-brightgreen.svg?colorB=89C967&logo=sensu)](https://bonsai.sensu.io/assets/SquadcastHub/sensu-squadcast-handler)
![Go Test](https://github.com/SquadcastHub/sensu-squadcast-handler/workflows/Go%20Test/badge.svg)
![goreleaser](https://github.com/SquadcastHub/sensu-squadcast-handler/workflows/goreleaser/badge.svg)

# Sensu Squadcast Handler

The Sensu Go Squadcast handler is a [Sensu Event Handler][1] that sends event data to
a Squadcast endpoint.

## Configuration

### Asset registration

Run the following command to add the asset `sensu-squadcast-handler`.

```shell
sensuctl asset add SquadcastHub/sensu-squadcast-handler
```

### Handler definition

Example Sensu Go handler definition:

**squadcast-handler.yaml**

```yaml
type: Handler
api_version: core/v2
metadata:
  name: squadcast
  namespace: default
spec:
  command: sensu-squadcast-handler
  env_vars:
  - SENSU_SQUADCAST_APIURL=<Squadcast Alert Source Url>
  runtime_assets:
  - SquadcastHub/sensu-squadcast-handler
  filters:
  - is_incident
  timeout: 10
  type: pipe
```

Run the following to create the handler:

```shell
sensuctl create -f squadcast-handler.yaml
```

Example Sensu Go check definition:

```yaml
api_version: core/v2
type: CheckConfig
metadata:
  namespace: default
  name: health-check
spec:
  command: check-http -u http://localhost:8080/health
  subscriptions:
  - test
  publish: true
  interval: 10
  handlers:
  - squadcast
```

## Usage examples

Help:

```
The Sensu Go Squadcast handler sends Sensu events to Squadcast


Usage:
  sensu-squadcast-handler [flags]


Flags:
  -a, --api-url string         The URL for the Squadcast API
  -h, --help                   help for sensu-squadcast-handler
  -e, --entity-id string       The template to use for the Entity ID (default "{{.Entity.Name}}/{{.Check.Name}}")
  -s, --state-message string   The template to use for the state message (default "{{.Entity.Name}}:{{.Check.Name}}:{{.Check.Output}}")
```

[1]: https://docs.sensu.io/sensu-go/5.0/reference/handlers/#how-do-sensu-handlers-work
