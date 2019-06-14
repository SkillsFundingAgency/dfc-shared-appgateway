# Application Gateway

Adds an application gateway

## parameters

appGatewayName: (required) string

Name of the application gateway resource to create

subnetRef: (required) string

The subnet resource ID where the application gateway will go.

If you have the vnet and subnet names, you can get the ID with the following function
`resourceId('Microsoft.Network/virtualNetworks/subnets', 'vnet', 'subnet')`

appGatewayTier: (optional) string

Application gateway type and instance size combined

Must be one of Standard_Small (default if none supplied), Standard_Medium, Standard_Large, WAF_Medium, WAF_Large, Standard_v2 or WAF_v2
                
backendPools: (required) array of object

A list of backend pools to create.
The first backend pool specified will be the default one (used if not routing)

Each backend is specified by an object consisting of

* name: the name the backend pool resource
* fqdn: the full domain name

An example of a valid object

```json
{
    "name": "backendName",
    "fqdn": "be.example.net"
}
```

routingRules: (required) array of object

A list of routing rules describing which paths should be routed to which backend pool.
If the path does not match any rule provided it will go to the default backend pool,
which is the first backend specified (see above).

Each backend is specified by an object consisting of

* name: the name the backend pool resource
* backend: the name of the backend to route to (as specified in the previous parameter, see above)
* paths: an array of paths, usually wildcarded, to route to the backend

An example of a valid object

```json
{
    "name": "routingRule",
    "backend": "backendName",
    "paths": [ "/myapp/*" ]
}
```

rewriteRules: (optional) array of object

A list of rewrite rules which will be applied.
If not specified, no rewrite rules will be specified.

Each rewrite rule is specified by an object consisting of

* name: the name the rewrite rule set
* actionSet: an array of objects specifying actionSet of the rule

An example of a valid object

```json
{
    "name": "rewriteRule",
    "actionSet": { ... }
}
```

See https://docs.microsoft.com/en-us/azure/templates/microsoft.network/2018-11-01/applicationgateways#ApplicationGatewayRewriteRuleActionSet

capacity: (optional) int

Number of instances of the application gateway to run.
Must be at least 2.
Defaults to 2 if not supplied.

privateIpAddress: (optional) string

Private IP address to allocate to the application gateway.
If not specified, no private IP address will be assigned to the app gateway.
At least one of privateIpAddress or publicIpAddressId must be supplied.

publicIpAddressId: (optional) string

An ID of a public IP address resource.
If not specified, no public IP address will be assigned to the app gateway.
At least one of privateIpAddress or publicIpAddressId must be supplied.

httpFrontendPort: (optional) int

The port the application gateway is accessible on via HTTP.
Defaults to port 80 if not specified.

httpsFrontendPort: (optional) int

The port the application gateway is accessible on via HTTP.
Defaults to port 443 if not specified.

backendPort: (optional) int

The backend port to access the backend pools over.
Defaults to port 80 if not specified.

backendProtocol: (optional) string

The protocol to access the backend pools over.
Must be either Http or Https.
Defaults to http if not specified.

cookieBasedAffinity: (optional) string

Enables or disables cookie based affinity.
Defaults to disabled

keyVaultName: (optional) string

Name of key vault to get the SSL certificate from.
Will only add SSL options if keyVaultName, keyVaultSecretName and userAssignedIdentityName are supplied.

keyVaultSecretName: (optional) string

Name of secret in key vault containing the SSL certificate.
Will only add SSL options if keyVaultName, keyVaultSecretName and userAssignedIdentityName are supplied.

userAssignedIdentityName: (optional) string

Name of assigned identity with secret read access to the key vault.
Will only add SSL options if keyVaultName, keyVaultSecretName and userAssignedIdentityName are supplied.

This can be created in ARM with a Microsoft.ManagedIdentity/userAssignedIdentities resource.
An example

```json
{
    "name": "myidentity",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
    "apiVersion": "2018-11-30",
    "location": "[resourceGroup().location]"
}
```