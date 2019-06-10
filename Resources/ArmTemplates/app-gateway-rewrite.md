# Application Gateway (with rewrites)

Adds an application gateway with rewrite rules

## parameters

appGatewayName

Name of the application gateway resource to create

subnetRef

tbc

appGatewayTier

Application gateway type and instance size combined

Must be one of Standard_Small (default if none supplied), Standard_Medium,
                "Standard_Large",
                "WAF_Medium",
                "WAF_Large",
                "Standard_v2",
                "WAF_v2
                
backendPools

routingRules