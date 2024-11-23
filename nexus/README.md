# Seznam poskytovaných API

| Jméno souboru | Stručný popis API |
| --- | --- |
| NexusDocker.yaml | mode: hosted: určuje, jak parametrizované bude Nexus repository pro lokální image |
| NexusDocker.yaml | mode: proxy: určuje, jak parametrizované bude Nexus repository pro image, které jsou pposkytov8ny pomocí proxy z internetu |
| NexusRoutingRule.yaml | Definuje parametry pro whitelisting, který je navázán na konkrétní Docker Proxy repository |
| Project.yaml | Popisuje jak bude vytvořen projekt v OpenShiftu |

# NexusDocker.yaml
curl -u <username>:<password> \
-X POST \
-H "Content-Type: application/json" \
-d '@JSON_DATA' \
https://nexus.pmb.cz/service/rest/v1/repositories/docker/hosted
<div style="display: flex; justify-content: space-between;">
<div style="width: 48%;">
<b>YAML</b>
<pre>
apiVersion: ppfbanka.cz/v1alpha1
kind: NexusDocker
metadata:
  name: docker-sandbox-hosted
spec:
  mode: hosted
  tcpPort: 6100
  hosted: {}
</pre>
</div>
<div style="width: 48%;">
<b>JSON</b>
<pre>
{
  "name": "docker-sandbox-hosted",
  "online": true,
  "storage": {
    "blobStoreName": "default",
    "strictContentTypeValidation": true,
    "writePolicy": "ALLOW"
  },
  "docker": {
    "v1Enabled": false,
    "forceBasicAuth": true,
    "httpPort": 6100,
    "httpsPort": null
  },
  "cleanup": {
    "policyNames": []
  },
  "component": {
    "proprietaryComponents": true
  }
}
</pre>
</div>
</div>

# NexusDocker.yaml
<div style="display: flex; justify-content: space-between;">
<div style="width: 48%;">
<b>YAML</b>
<pre>
apiVersion: ppfbanka.cz/v1alpha1
kind: NexusDocker
metadata:
  name: docker-quay-io-proxy
spec:
  mode: proxy
  tcpPort: 6001
  proxy:
    sourceUrl: https://quay.io
    routingRuleName: quay-io-whitelist
    authentication:
      username: username
      password: password
</pre>
</div>
<div style="width: 48%;">
<b>JSON</b>
<pre>
{
  "name": "Example",
  "version": "1.0",
  "dependencies": [
    {
      "package": "sample",
      "version": "2.1"
    }
  ]
}
</pre>
</div>
</div>

# NexusRoutingRule.yaml
<div style="display: flex; justify-content: space-between;">
<div style="width: 48%;">
<b>YAML</b>
<pre>
apiVersion: ppfbanka.cz/v1alpha1
kind: NexusRoutingRule
metadata:
  name: quay-io-whitelist
spec:
  mode: Block
  matchers:
  - "^/v2/openshift-release-dev/.*$"
</pre>
</div>
<div style="width: 48%;">
<b>JSON</b>
<pre>
{
  "name": "Example",
  "version": "1.0",
  "dependencies": [
    {
      "package": "sample",
      "version": "2.1"
    }
  ]
}
</pre>
</div>
</div>

# Project.yaml
Automatizace: ArgoCD + [HelmChart](https://gitlab.pmb.cz/ocp_clusters/projects-onboarding/-/tree/master/helmchart?ref_type=heads)
