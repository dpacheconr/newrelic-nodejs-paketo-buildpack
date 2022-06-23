
# newrelic-nodejs-paketo-buildpack
## The Paketo New Relic Agent Buildpack is a Cloud Native Buildpack that contributes and configures the New Relic NodeJs Agent.

  

**Behavior**

  
This buildpack will participate if all the following conditions are met

<br/>
The `$BP_NEW_RELIC_ENABLED` is set to true (defaults to false)
<br/><br/>

The buildpack will do the following for NodeJs applications:
<br/>
Installs New Relic Nodejs agent using npm
<br/>
Contributes the New Relic NodeJs Agent to the newrelic-nodejs layer configures `NODE_PATH` to include the added modules
<br/>
Copies New Relic NodeJs Agent configuration file in /resources/newrelic.js to the root folder of your application
<br/>
Configures your main application module with `require('newrelic');`
<br/><br/>


**Variables**
<br/>

| Key | Description |
|--|--|
| `BP_NEW_RELIC_ENABLED` | Defaults to false - will not participate - as set in buildpack.toml   |
| `NEW_RELIC_APP_NAME` | Defaults to app_name variable set in newrelic.js  |
| `NEW_RELIC_LICENSE_KEY`  | Required at build time or runtime     |
| `NEW_RELIC_AGENT_ENABLED`  | Defaults to agent_enabled variable set in newrelic.js |

<br/>
You can override any setting from a system property or in the newrelic.js by setting an environment variable.

The environment variable corresponding to a given setting in the config file is the setting name prefixed by NEW_RELIC with all dots (.) and dashes (-) replaced by underscores (_). 

For this to work as part of the **build stage**, you will need to precede the variables with BPE, i.e. `BPE_NEW_RELIC_LOG_LEVEL`

For this to work during **runtime**, simply prefix the setting name, i.e. `NEW_RELIC_LOG_LEVEL`

Please refer New Relic Nodejs agent documentation for more information

https://docs.newrelic.com/docs/apm/agents/nodejs-agent/installation-configuration/nodejs-agent-configuration/#environment

<br/>

**Example**
  
pack build CONTAINERNAME -p PATHTOYOURAPPLICATION -b paketo-buildpacks/nodejs -b PATHTOROOTFOLDEROFYOURBUILDPACK ... \

--env `BP_NEW_RELIC_ENABLED`=true \ -----------> Required condition to build the buildpack as default is false

--env `BPE_NEW_RELIC_APP_NAME`=xxxxxxxxxx \ -----------> Optional can be set on runtime

--env `BPE_NEW_RELIC_LICENSE_KEY`=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx -----------> Optional can be set on runtime

 <br/>

By default the NodeJS agent configuration file will be located at your application root folder, this can be overwritten at runtime, with configmap for kubernetes deployments.

Please refer to Kubernetes documentation for more information about configmaps

[https://kubernetes.io/docs/concepts/configuration/configmap/](https://kubernetes.io/docs/concepts/configuration/configmap/)
