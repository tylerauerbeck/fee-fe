# Development on OpenShift

## Getting Started With Helm

This directory contains a Helm chart which can be used to deploy a development version of this app for rapid testing.

Before you use it, you will need to download & install Helm 3.

If you are not familiar with Helm - how to configure it and run - you can start with this quickstart:

[https://helm.sh/docs/intro/quickstart](https://helm.sh/docs/intro/quickstart)

## Using This Chart

1. Clone the target repo:

```
git clone https://github.com/rht-labs/fee-fe.git
```

2. Change into to the `deployment` directory:

```
cd fee-fe/deployment
```

3. Deploy using the following Helm command:

```shell script
helm template . \
  --values values-dev.yaml \
  --set git.uri=https://github.com/rht-labs/fee-fe.git \
  --set git.ref=master \
  --set baseUrl=http://fee-fe-fe-enablement-dev.apps.s44.core.rht-labs.com \
   --set hostname=fee-fe.apps.s44.core.rht-labs.com \
| oc apply -f -
```

It accepts the following variables

| Variable  | Use  |
|---|---|
| `git.uri`  | The HTTPS reference to the repo (your fork!) to build  |
| `git.ref`  | The branch name to build  |
| `baseUrl`  | The FQDN at which this route will be exposed - depends on your environment  |
| `clientId`  | The client ID that the SSO server is configured to accept auth requests using  |
| `authBaseUrl`  | The url that your SSO server accepts OpenID Connect requests on - for Keycloak, something like `https://<keycloak-base-url>.com/auth/realms/<realm-id>/protocol/openid-connect`  |
| `backendUrl`  | The url that the LodeStar backend accepts requests on  |

This will spin up all of the usual resources that this service needs in production, plus a `BuildConfig` configured to build it from source from the Git repository specified. To trigger this build, use `oc start-build omp-frontend`.

**Note**: Also check out the list of runtime variables in the [top level README](../README.md#runtime-configuration-variables)
