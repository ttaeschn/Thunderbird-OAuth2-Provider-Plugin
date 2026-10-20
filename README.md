# Thunderbird OAuth Provider Extension

This plugin for Thunderbird adds a custom OAuth2 Provider (in addition to the default providers Microsoft and Google). 

## Background
Mozilla Thunderbird has built support for OAuth2. Unfortunately the necessary configuration parameters are not directly configurable in Thunderbird and only parameters for some public OAuth2 Providers like Google or Microsoft are build into the application directly. Details see https://support.mozilla.org/en-US/questions/1530032

Since Thunderbird Version 140.0.0 an API to register and unregister custom providers is able which makes it possible to add your custom provider via API/Extension.

Since Thunderbird 157 this API is built into Thunderbird as the `oauth_provider` manifest key, and its internal signature changed. Version 1.1 of this extension therefore only contains a `manifest.json` and needs Thunderbird 157 or newer. Version 1.0 (with its own experiment API) fails silently on Thunderbird 157 and Thunderbird reports that the authentication method is not supported.

## Configuration
You need to update the values of *oauth_provider* to match your provider. If available, take the values from your .well-known endpoint. E.g. https://<provider-url>/realms/<realm-name>/.well-known/openid-configuration


**issuer** : name of your provider. This <em>should</em> match the hostname of the authorization endpoint, although that is not required. You can safely take issuer value from the .well-known endpoint of your provider

**clientId** : the id of the client you registered with the provider

**clientSecret**: the secret for the clienId as configured in the provider

**issuerIdentifier**: take "issuer" from .well-known, e.g. "https://<provider-url>/realms/<realm-name>". Keycloak returns this value as `iss` after login (RFC 9207) and Thunderbird checks it.

**authorizationEndpoint**: take "authorization_endpoint" from .well-known E.g. "https://<provider-url>/realms/<realm-name>/protocol/openid-connect/auth"

**tokenEndpoint**: take "token_endpoint" from .well-known. E.g. "https://<provider-url>/realms/<realm-name>/protocol/openid-connect/token",
    
**redirectionEndpoint**: for this plugin leave the value at "https://localhost" 

**usePKCE**: false -- Whether the provider and the configured client supports PKCE. *I have not tested PKCE!. So change at your own risk* 

**hostnames**: list of hostnames that use this provider, this must match SMTP/IMAP/POP server when configuring Thunderbird (you can add many as an array).

**scopes**: the scopes to request from the provider e.g. "email profile"


## Building the plugin
You can either manually zip the necessary or use web-ext build to package the addon
### using web-ext
You need to install nodejs and have web-ext installed.  
Simply run `web-ext build` or `npx web-ext build` in the base directory of this repository. 
This will create the addon-on as xpi-archive in the folder web-ext-artifacts.  

### Manual build
Zip the necessary files into an xpi archive;
`mkdir -p web-ext-artifacts && zip -r web-ext-artifacts/thunderbird_oauth_provider_extension-1.0.xpi icons/ experiments/ manifest.json`

## Installing the plugin
To install the plugin (the xpi archive) you have to choose the "Install Add-on From File" Method mentioned in the Mozilla Support [link](https://support.mozilla.org/en-US/kb/installing-addon-thunderbird#w_a-slightly-less-ideal-case-install-from-a-downloaded-xpi-file) and select the XPI/ZIP archive and allows it to have full access to Thunderbird.

## Provenance and Contributions
This add-on is based on original Mozilla source code testing the API OAuth2Providers. Special thanks to [darktrojan](https://github.com/darktrojan) for the Mozilla implementation. 

Here is the link to the Mozilla Thunderbird Changeset: [Phabricator link](https://phabricator.services.mozilla.com/D250109#change-knYSrVKOhFUz)

The icons of this addon were generated with the assistance of AI tools and subsequently reviewd and integrated by human contributors. As was some of the spelling. 



