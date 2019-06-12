# Application Gateway (with rewrites)

Adds an application gateway with rewrite rules

## parameters

appGatewayName: (required) string

Name of the application gateway resource to create

subnetRef: (required) string

The subnet resource ID where the application gateway will go.

If you have the vnet and subnet names, you can get the ID with the following function
`resourceId('Microsoft.Network/virtualNetworks/subnets', 'vnet', 'subnet')`

appGatewayTier: (optional) string

Application gateway type and instance size

Must be either Standard_v2 (the default if not is supplied) or WAF_v2
                
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

capacity: (optional) int

Number of instances of the application gateway to run.
Must be at least 2.
Defaults to 2 if not supplied.

privateIpAddress: (optional) string

Private IP address to allocate to the application gateway.
If not specified, a public IP address will be created instead.
The private IP address must be in the range specified by the subnet.

publicIpAddressId: (optional) string

Public IP address reference ID.
Does not allocate a public IP address if not specified.

If you have the name of the public IP address resource you can get the ID with the following function
`resourceId('Microsoft.Network/publicIPAddresses', 'publicIPAddressName')`.

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

keyVaultName, keyVaultSecretName & userAssignedIdentityName: (optional) string

Not yet used. Will be used to set the SSL certificate.