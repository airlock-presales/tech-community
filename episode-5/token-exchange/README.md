# Airlock Tech Community - Token Exchange

The 5th episode of the Airlock Tech Community Trainings explained and demonstrated how Token Exchange works. The Airlock Microgateway transparently exchanged OAuth2 or OIDC access or ID tokens for an application to:

- Harmonise different tokens from multiple issuers on a single internal standard.
- Eliminate the *almighty* single token accepted by all services and replace it with service-individual tokens.

**If you think tokens, think Airlock.**

## Setup demo environment

Prerequisite:
- Airlock Microgateway installed according to the [Running Example](https://github.com/airlock/microgateway-running-example)

Installation

1. Create namespace for demo
   ```
   kubectl create ns entra
   ```
1. Substitute the POD IP network mask in <code>deployment.yaml</code>
   ```
   - name: HTTPSHOW_TRUSTED_PROXIES
     value: "10.42.0.0/16"
   ```

   If you don't know it, use one of the following methods:
   1. Deduct from pod IP addresses
      ```
      kubectl -n airlock-gateway get pod -o wide | awk '/airlock/{print $6}'
      ```
   2. Extract from cluster info
      ```
      kubectl cluster-info dump | jq -r '.items[]|.spec.podCIDR'
      ```

1. Update the Microsoft Entra ID tenant id in <code>secret.yaml</code>
   ```
   HTTPSHOW_OIDC_ISSUER: https://login.microsoftonline.com/<tenant-uuid>/v2.0
   HTTPSHOW_OIDC_LOGOUT_URL: https://login.microsoftonline.com/<tenant-uuid>/oauth2/v2.0/logout
   ```
1. Set client id and secret to connect to Microsoft Entra ID in <code>secret.yaml</code>
   ```
   HTTPSHOW_OIDC_CLIENT_ID: <client-uuid>
   HTTPSHOW_OIDC_CLIENT_SECRET: <client-secret>
   ```
1. Apply manifests
   ```
   kubectl apply -k .
   ```
1. For the relevant IAM snippets, go to [here](https://github.com/airlock-presales/iam-snippets) and check:
   - OIDC OP - it includes the Token Exchange Service
