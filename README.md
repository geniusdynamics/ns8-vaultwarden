# ns8-vaultwarden
vaultwarden is a community-supported open-source document management system that transforms your physical documents into a searchable online archive so you can keep, well, less paper.

## Install

Instantiate the module with:

   ```shell
    add-module ghcr.io/compgeniuses/vaultwarden:latest 1
   ```

The output of the command will return the instance name.
Output example:

    {"module_id": "vaultwarden", "image_name": "vaultwarden", "image_url": "ghcr.io/compgeniuses/vaultwarden:latest"}
## Update Module

```shell
api-cli run update-module --data '{"module_url":"ghcr.io/compgeniuses/vaultwarden:latest","instances":["vaultwarden"],"force":true}'
```
## Configure

Let's assume that the vaultwarden instance is named `vaultwarden1`.

Launch `configure-module`, by setting the following parameters:

- `LOGIN_RATELIMIT_MAX_BURST`: 10
- `LOGIN_RATELIMIT_SECONDS`: 60
- `ADMIN_RATELIMIT_MAX_BURST`: 10
- `ADMIN_RATELIMIT_SECONDS`: 60
- `ADMIN_TOKEN`: YourReallyStrongAdminTokenHere
- `SENDS_ALLOWED`: true
- `EMERGENCY_ACCESS_ALLOWED`: true
- `WEB_VAULT_ENABLED`: true
- `SIGNUPS_ALLOWED`: false
- `SIGNUPS_VERIFY`: true
- `SIGNUPS_VERIFY_RESEND_TIME`: 3600
- `SIGNUPS_VERIFY_RESEND_LIMIT`: 5
- `SIGNUPS_DOMAINS_WHITELIST`: yourdomainhere.com,anotherdomain.com
- `SMTP_HOST`: smtp.youremaildomain.com
- `SMTP_FROM`: vaultwarden@youremaildomain.com
- `SMTP_FROM_NAME`: Vaultwarden
- `SMTP_SECURITY`: SECURITYMETHOD
- `SMTP_PORT`: XXXX
- `SMTP_USERNAME`: vaultwarden@youremaildomain.com
- `SMTP_PASSWORD`: YourReallyStrongPasswordHere
- `SMTP_AUTH_MECHANISM`: "Mechanism"
- `lets_encrypt`: Set LEtsecnrypt to True or False, Default is FALSE
- `http2https`: set redirect to True or False, Default is True
- `host`: the traefik host url for the will be DOMAIN=https://vaultwarden.yourdomain.com

- ...

Example:

    api-cli run module/vaultwarden1/configure-module --data '{"host": "vaultwarden.domain.com"}'

    or if modifying another value: 

    api-cli run module/vaultwarden5/configure-module --data '{"host": "vaultwarden.domain.com","vaultwarden_name": "Myvaultwarden"}'

    api-cli run module/vaultwarden1/configure-module --data '{
        "host": "papperles.rocky9-pve2.org",
        "lets_encrypt": false,
        "http2https": true,
        "WEB_VAULT_ENABLED": true,
        "SIGNUPS_ALLOWED": fales,
        "SIGNUPS_DOMAINS_WHITELIST":"yourdomainhere.com,anotherdomain.com",
        "ADMIN_TOKEN":"YourReallyStrongAdminTokenHere"
    }'


The above command will:
- start and configure the vaultwarden instance
- (describe configuration process)
- ...

Additional Parameters are Described here:
https://github.com/dani-garcia/vaultwarden/wiki
WHile they have not been Implemented, if you require more parameters to be defined, kindly free to raise an issue, and define why and how that parameter should be implemented for use

Send a test HTTP request to the vaultwarden backend service:

    curl http://127.0.0.1/vaultwarden/

## Smarthost setting discovery

Some configuration settings, like the smarthost setup, are not part of the
`configure-module` action input: they are discovered by looking at some
Redis keys.  To ensure the module is always up-to-date with the
centralized [smarthost
setup](https://nethserver.github.io/ns8-core/core/smarthost/) every time
kickstart starts, the command `bin/discover-smarthost` runs and refreshes
the `state/smarthost.env` file with fresh values from Redis.

Furthermore if smarthost setup is changed when vaultwarden is already
running, the event handler `events/smarthost-changed/10reload_services`
restarts the main module service.

See also the `systemd/user/vaultwarden-server.service` file.

This setting discovery is just an example to understand how the module is
expected to work: it can be rewritten or discarded completely.

## Uninstall

To uninstall the instance:

    remove-module --no-preserve vaultwarden1

## Testing

Test the module using the `test-module.sh` script:


    ./test-module.sh <NODE_ADDR> ghcr.io/nethserver/vaultwarden:latest

The tests are made using [Robot Framework](https://robotframework.org/)

## UI translation

Translated with [Weblate](https://hosted.weblate.org/projects/ns8/).

To setup the translation process:

- add [GitHub Weblate app](https://docs.weblate.org/en/latest/admin/continuous.html#github-setup) to your repository
- add your repository to [hosted.weblate.org]((https://hosted.weblate.org) or ask a NethServer developer to add it to ns8 Weblate project

## To Do

Implement Ldap Sync using these modules
https://hub.docker.com/r/vividboarder/vaultwarden_ldap

it includes alot of parameters

if not implemented we could use this

https://github.com/bitwarden/directory-connector

this docker image seems to pre-implement SSO https://github.com/Timshel/vaultwarden/pkgs/container/vaultwarden

Also this pre-implemnts SSO: https://hub.docker.com/r/oidcwarden/vaultwarden-oidc/tags

SSO PR seemed to be in the worsk here as well: https://github.com/dani-garcia/vaultwarden/pull/3899
so would be rebased, once its ready