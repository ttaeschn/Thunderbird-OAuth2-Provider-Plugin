# Thunderbird OAuth Provider Extension

This plugin for Thunderbird adds a custom OAuth2 Provider (in addition to the default providers Microsoft and Google). 

## Background
Mozilla Thunderbird has built support for OAuth2. Unfortunately the necessary configuration parameters are not directly configurable in Thunderbird and only parameters for some public OAuth2 Providers like Google or Microsoft are build into the application directly. Details see https://support.mozilla.org/en-US/questions/1530032

Since Thunderbird Version 140.0.0 an API to register and unregister cutom providers is able which makes it possible to add your custom provider via API/Extension.

## Configuration
You need to update the values of *oauth_provider* to match your provider. If available take the values from your .well-known endpoint. E.g. https://<provider-url>/realms/<realm-name>/.well-known/openid-configuration


**issuer** : name of your provider. This <em>should</em> match the hostname of the authorization endpoint, although that is not required. You can safely take issuer value from the .well-known endpoint of your provider

**clientId** : the id of the client you registered with the provider

**clientSecret**: the secret for the clienId as configured in the provider

**authorizationEndpoint**: take "authorization_endpoint" from .well-known E.g. "https://<provider-url>/realms/<realm-name>/protocol/openid-connect/auth"

**tokenEndpoint**: take "token_endpoint" from .well-known. E.g. "https://<provider-url>/realms/<realm-name>/protocol/openid-connect/token",
    
**redirectionEndpoint**: for this plugin leave the value at "https://localhost" 

**usePKCE**: false -- Whether the provider and the configured client supports PKCE. *I have not tested PKCE!. So change at your own risk* 

**hostnames**: list of hostnames that use this provider

**scopes**: the scopes to request from the provider e.g. "email profile"

## Building the plugin
You can either manually zip the necessary or use web-ext build to package the addon
### using web-ext
You need to install have nodejs and web-ext installed.  
Simple run `web-ext build` or `npx web-ext build` in the base directory of this repository. 
This will create the addon-on as xpi-archive in the folder web-ext-artifacts.  

### manually
simple zip the necessary files into an xpi archive. 
E.g. `zip -r web-ext-artifacts/thunderbird_oauth_provider_extension-1.0.xpi icons/ experiments/ manifest.json`

## Installing the plugin
To install the plugin (the xpi archive) you have to choose the "Install Add-on From File" Method mentioned in the Mozilla Support [link](https://support.mozilla.org/en-US/kb/installing-addon-thunderbird#w_a-slightly-less-ideal-case-install-from-a-downloaded-xpi-file). 

## Provenance and Contributions
This add-on is based on original Mozilla source code testing the API OAuth2Providers. Special thanks to [darktrojan](https://github.com/darktrojan) for the Mozilla implementation. 

Here is the link to the Mozilla Thunderbird Changeset: [Phabricator link](https://phabricator.services.mozilla.com/D250109#change-knYSrVKOhFUz)

The icons of this addon where generated with the assistance of AI tools and subsequently reviewd and integrated by human contributors.”



