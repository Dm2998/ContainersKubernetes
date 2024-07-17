# Containers Kubernetes
Containers Doc SnowAltas 


```
Set up container connectors
Container connectors enable data to be collected and transferred from your Kubernetes environment to Snow Atlas. You create container connectors in Snow Atlas and use Helm charts to install them in your clusters.

For container connectors to collect data from your Kubernetes clusters, you must create and install a connector for each cluster.

Create container connectors
You must create a new connector for each Kubernetes environment. When you create a connector, you generate a new secret key that you must use with the Helm chart to install the connector in your environment. The secret key includes the client ID, client secret and Snow Atlas connection address.

On the Settings menu, under Container settings, select Container connectors.

Select Add connector.

In Container connector name, enter a name for the connector that identifies the cluster in which you will install the connector.

Optional: In Description, enter additional information about the connector.

Select your enrollment site from the Enrollment site list.

Select Generate secret key.

Select Copy and save the key in a secure location.

NOTE
You cannot restore a secret key. If you lose the secret key and want to install this connector, you must regenerate a secret key and use that one instead. For more information, see Generate new keys.

Select Finish.

Install container connectors with default values
Use this procedure for a standard installation.

When you install the snowsoftware-connector-k8s Helm chart from the repository github.com/SnowSoftwareGlobal/helm-charts , you require the secret key that you generate and copy in Create container connectors.

Add the repository https://snowsoftwareglobal.github.io/helm-charts to your Helm chart repositories:

helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update

Install the connector with the latest version and paste in the secret key that you generated:

helm install snowsoftware-connector snowsoftware/snowsoftware-connector-k8s --set secretKey=[secretkey]

Install container connectors with custom certificates
Use this procedure if you want to include custom certificates when you install the Helm chart. You must include entries for a tls.cert and tls.key for the agent, aggregator and message queue.

To add your certificates, you require a secrets management tool.

When you install the snowsoftware-connector-k8s Helm chart from the repository github.com/SnowSoftwareGlobal/helm-charts , you require the secret key that you generate and copy in Create container connectors.

Generate the certificates that you want to include and add them as secrets to your chosen secrets management tool.

Add the repository https://snowsoftwareglobal.github.io/helm-charts to your Helm chart repositories:

helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update

Install the connector and provide the name of the secret key for your secrets management tool and the certificate secrets:

helm install snowsoftware-connector snowsoftware/snowsoftware-connector-k8s \
       --set existingSecretKey=[name of secretkey in k8s] \
       --set existingAgentCertificate=[name of agent certificate secret in k8s] \
       --set existingAggregatorCertificate=[name of agent certificate secret in k8s] \
       --set existingMQCertificate=[name of agent certificate secret in k8s]

Install container connectors with amended values
Use this procedure if you want to install connectors with an amended values.yaml file. For the values that you can override, see Configurable values for container connectors.

When you install the snowsoftware-connector-k8s Helm chart from the repository github.com/SnowSoftwareGlobal/helm-charts , you require the secret key that you generate and copy in Create container connectors.

Copy the values.yaml file from https://github.com/SnowSoftwareGlobal/helm-charts/charts/snowsoftware-connector-k8s/values.yaml  and amend it as you require. For information on the values that you can override, see Configurable values for container connectors.

Add the repository https://snowsoftwareglobal.github.io/helm-charts to your Helm chart repositories:

helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update

Install the connector and include the amended values.yaml file:

helm install snowsoftware-connector \ 
      -f values.yaml \
      snowsoftware/snowsoftware-connector-k8s

Install container connectors with custom namespace
Use this procedure if you want to include a custom namespace when you install the Helm chart.

When you install the snowsoftware-connector-k8s Helm chart from the repository github.com/SnowSoftwareGlobal/helm-charts , you require the secret key that you generate and copy in Create container connectors.

Add the repository https://snowsoftwareglobal.github.io/helm-charts to your Helm chart repositories:

helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update

Install the connector with the latest version, enter the namespace, and paste in the secret key that you generated:

helm install -n <namespace> snowsoftware-connector snowsoftware/snowsoftware-connector-k8s --set secretKey=<secretKey>


NOTE
If your Kubernetes distribution is Red Hat OpenShift, you must change the SCC policy from restricted-v2 to nonroot-v2. To do this, after you install the connector, run the following in your Red Hat OpenShift cluster:

oc adm policy add-scc-to-user nonroot-v2 -z default -n [namespace]
oc adm policy add-scc-to-user nonroot-v2 -z snow-account -n [namespace]

Upgrade container connectors
Load all the new charts from the repository github.com/SnowSoftwareGlobal/helm-charts :

helm repo update

View the charts available:

helm search repo

List what is currently installed:

helm list

Do one of the following:

Upgrade to a specific version:

helm upgrade snowsoftware-connector snowsoftware/snowsoftware-connector-k8s \
      --version [release version]

Upgrade to the latest version:

helm upgrade snowsoftware-connector snowsoftware/snowsoftware-connector-k8s

NOTE
If you make changes to the values.yaml file, you must upgrade the helm charts to apply your changes:

helm upgrade snow-connector \        
      -f values.yaml \       
      snowsoftware/snowsoftware-connector-k8

Snow Software does not own the third party trademarks, software, products, or tools (collectively, the "Third Party Products") referenced herein. Third Party Product updates, including user interface updates, may not be reflected in this content.

