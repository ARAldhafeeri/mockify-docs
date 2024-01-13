
# View Auto-Generated Endpoints

Mockify auto-generates mock API endpoints based on two user actions.

## 1. Auto-generated generics based on resource definition
![Endpoint](https://github.com/ARAldhafeeri/mockify-docs/blob/main/imgs/endpointres.png?raw=true)
Based on resource creation and feature flagging in resource creation, Mockify auto-generates endpoints for those mocked resources. You can view them at the "Endpoint" tab, labeled as generic. The schema for the URL is as follows:

```plaintext
https://<<domainName>>/mock/<resourceName>
```
For example, if getx and putx in resource definition are enabled, then the following endpoints will be auto-generated:
   
```plaintext
GET -> https://<<domainName>>/mock/<resourceName>
PUT -> https://<<domainName>>/mock/<resourceName>
```

Note: Each string enclosed with `< >` is a parameter.

## 2. Auto-generated based on edge function definition
![Endpoint](https://github.com/ARAldhafeeri/mockify-docs/blob/main/imgs/endpointedge.png?raw=true)
Based on the definition of an edge function, Mockify auto-generates endpoints when an edge function is created. The schema for the URL is as follows:

```plaintext
https://<<domainName>>/<resourceName>/edge/<edgeFunctionName>
```
Note: : function feature flag should be enabled in the resource definition. Also, the method the edge function uses should be enabled in the resource definition.

For example, if getx functions in resource definition are enabled, then we can create edge functions as follows:
name -> test
method -> GET
code -> code
resource -> resource

Then the following endpoints will be auto-generated:
   
```plaintext
GET -> https://<<domainName>>/<resourceName>/edge/test
```

Note: Each string enclosed with `< >` is a URL parameter.
