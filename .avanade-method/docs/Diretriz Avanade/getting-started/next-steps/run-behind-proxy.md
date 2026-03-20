# Run Tooling behind a Proxy

Executing any tools behind a proxy, either from a virtualized workstation or within a private build server, requires additional configuration.
You can find additional details [here](../../../Pipelines/Self-Hosted-Agent-Setup) on how to provide additional proxy information.

Most of the client tools (EMC, Deployment Tool, DMA) are supporting a proxy parameter `-Proxy:`:
```
dma.exe "ApplyGeneralSettings.xml" "-CrmConnectionString:AuthType=ClientSecret;Url=https:://customer1-build.crm4.dynamics.com/; ClientId=66666666-5555-4444-3333-222222222222; ClientSecret=SECRET;" "-Proxy:http:://10.10.10.10:8080"
```

!!! note 

    The two colons `::` in the URL are required to distinguish between parameters and the https specific separator.