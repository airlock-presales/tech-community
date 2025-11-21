# Airlock Tech Community - Token Exchange

The 5th episode of the Airlock Tech Community Trainings explained and demonstrated how Token Exchange works. The Airlock Microgateway transparently exchanged OAuth2 or OIDC access or ID tokens for an application to:

- Harmonise different tokens from multiple issuers on a single internal standard.
- Eliminate the *almighty* single token accepted by all services and replace it with service-individual tokens.

**If you think tokens, think Airlock.**

## Setup demo environment

Prerequisite:
- Airlock Microgateway installed according to the [Running Example](https://github.com/airlock/microgateway-running-example)

Installation

1. Substitute the POD IP network mask in <code>entra/deployment.yaml</code> and <code>api/deployment.yaml</code>
   ```
   - name: HTTPSHOW_TRUSTED_PROXIES
     value: "10.42.0.0/16"
   ```

   If you don't know it, use one of the following methods:
   - Deduct from pod IP addresses
      ```
      kubectl -n airlock-gateway get pod -o wide | awk '/airlock/{print $6}'
      ```
   - Extract from cluster info
      ```
      kubectl cluster-info dump | jq -r '.items[]|.spec.podCIDR'
      ```

1. Update the Microsoft Entra ID tenant id and client id and secret in <code>entra/secret.yaml</code>
   ```
   HTTPSHOW_OIDC_ISSUER: https://login.microsoftonline.com/<tenant-uuid>/v2.0
   HTTPSHOW_OIDC_LOGOUT_URL: https://login.microsoftonline.com/<tenant-uuid>/oauth2/v2.0/logout
   HTTPSHOW_OIDC_CLIENT_ID: <client-uuid>
   HTTPSHOW_OIDC_CLIENT_SECRET: <client-secret>
   ```
1. Update the Microsoft Entra ID tenant id in <code>api/jwks-entra.yaml</code>
   ```
   uri: https://login.microsoftonline.com/<tenant-uuid>/discovery/v2.0/keys
   ```
1. Update the Microsoft Entra ID tenant id in <code>api/jwt.yaml</code>
   ```
   issuer: https://login.microsoftonline.com/<tenant-uuid>/v2.0
   ```
1. Update the client secret to match the static client configured in your Airlock IAM instance in <code>api/secret.yaml</code>
   ```
   client.secret: password
   ```
1. Update the URL to point to your Airlock IAM instance token exchange endpoint in <code>api/tokenexchange.yaml</code>
   ```
   uri: https://<host>/<instance>-login/rest/oauth2/authorization-servers/<as-id>/token
   ```
1. Apply manifests
   ```
   kubectl apply -k api
   kubectl apply -k entra
   ```
1. For the relevant IAM snippets, go to [here](https://github.com/airlock-presales/iam-snippets) and check:
   - OIDC OP - it includes the Token Exchange Service

## Show the demo

Entrypoint for the demo is: https://entrashow-127-0-0-1.nip.io/
