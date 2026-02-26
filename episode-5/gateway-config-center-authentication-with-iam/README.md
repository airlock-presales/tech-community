# Airlock Tech Community - Airlock Gateway Config Center Authentication with Airlock IAM

In the 5th episode of the Airlock Tech Community Trainings, we explained and demonstrated how to use Airlock IAM to authenticate admins to login to the Airlock Gateway Config Center.

This snippet and the full blown configuration implementing a OIDC RP using an OIDC OP for e.g. SSO, both for Airlock IAM 8.5, implement the example configuration documented in https://docs.airlock.com/gateway/latest/index/1635400835116.html.

## Setup your Airlock Gateway

1. Check and set values in /opt/airlock/custom-settings/mgt-tomcat/java-options.properties
2. Restart service: systemctl restart airlock-mgt-tomcat.service

## Setup your Airlock IAM

1. Use your preferred config, e.g. the IAM start config as the base configuration.
2. Create a new Target Application in the Loginapp and use the preconfigured plugin as a new Target Application. 
3. Add your preferred authentication flow to this new Target Application
4. Create a new Authentication UI in the Loginapp and use the preconfigured plugin as a new UI.
5. Set the preconfigured Allowed URI Pattern the UI - On Logout - Query Parameter URI Extractor.
6. Search for "company.com" and set the appropriate values for hostname and domain for both Gateway and IAM.

Try to login into your Airlock Gateway Config Center! You should be redirected to your Airlock IAM and after authenticating yourself end up in the Airlock Gateway Config Center.
