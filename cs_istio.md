---

copyright:
  years: 2014, 2020
lastupdated: "2020-08-27"

keywords: kubernetes, iks, envoy, sidecar, mesh, bookinfo

subcollection: containers

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Setting up Istio
{: #istio}

Istio on {{site.data.keyword.containerlong}} provides a seamless installation of Istio, automatic updates and lifecycle management of Istio control plane components, and integration with platform logging and monitoring tools.
{: shortdesc}

With one click, you can get all Istio core components up and running. Istio on {{site.data.keyword.containerlong_notm}} is offered as a managed add-on so that {{site.data.keyword.cloud_notm}} automatically keeps all your Istio components up-to-date.

## Installing the Istio add-on
{: #istio_install}

In Kubernetes version 1.16 and later clusters, you can install the generally available managed Istio add-on.
{: shortdesc}

**Before you begin**</br>
* Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service role](/docs/containers?topic=containers-users#platform) for {{site.data.keyword.containerlong_notm}}.
* [Create a standard Kubernetes version 1.16 or later cluster with at least 3 worker nodes that each have 4 cores and 16 GB memory (`b3c.4x16`) or more](/docs/containers?topic=containers-clusters#clusters_ui). To use an existing cluster, you must update both the [cluster master to version 1.16](/docs/containers?topic=containers-update#master) and the [worker nodes to version 1.16](/docs/containers?topic=containers-update#worker_node).
* You cannot run community Istio concurrently with the managed Istio add-on in your cluster. If you use an existing cluster and you previously installed Istio in the cluster by using the IBM Helm chart or through another method, [clean up that Istio installation](#istio_uninstall_other).
* Classic multizone clusters: Ensure that you enable a [Virtual Router Function (VRF)](/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) for your IBM Cloud infrastructure account. To enable VRF, [contact your IBM Cloud infrastructure account representative](/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#benefits-of-moving-to-vrf). To check whether a VRF is already enabled, use the `ibmcloud account show` command. If you cannot or do not want to enable VRF, enable [VLAN spanning](/docs/vlans?topic=vlans-vlan-spanning#vlan-spanning). To perform this action, you need the **Network > Manage Network VLAN Spanning** [infrastructure permission](/docs/containers?topic=containers-users#infra_access), or you can request the account owner to enable it. To check whether VLAN spanning is already enabled, use the `ibmcloud ks vlan spanning get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).

**To use the {{site.data.keyword.cloud_notm}} console:**

1. In your [cluster dashboard](https://cloud.ibm.com/kubernetes/clusters){: external}, click the name of the cluster where you want to install the Istio add-on.

2. Click the **Add-ons** tab.

3. On the Managed Istio card, click **Install**.

4. Click **Install** again.

5. On the Managed Istio card, verify that the add-on is listed.

**To use the CLI:**

1. [Target the CLI to your cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

2. Enable the `istio` add-on. The default version of the generally available Istio managed add-on, 1.6.8, is installed.
  ```
  ibmcloud ks cluster addon enable istio --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Verify that the managed Istio add-on has a status of `Addon Ready`.
  ```
  ibmcloud ks cluster addon ls --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Example output:
  ```
  Name            Version     Health State   Health Status
  istio           1.6.8       normal         Addon Ready
  ```
  {: screen}

4. You can also check out the individual components of the add-on to ensure that the Istio services and their corresponding pods are deployed.
    ```
    kubectl get svc -n istio-system
    ```
    {: pre}

    ```
    kubectl get pods -n istio-system
    ```
    {: pre}

5. Next, you can include your apps in the [Istio service mesh](/docs/containers?topic=containers-istio-mesh#istio_sidecar) or set up visualization of your Istio mesh by [creating a secret for Kiali](/docs/containers?topic=containers-istio-health#kiali).

<br />


## Installing the `istioctl` CLI
{: #istioctl}

Install the `istioctl` CLI client. For more information, see the [`istioctl` command reference](https://istio.io/latest/docs/reference/commands/istioctl/){: external}.

1. Check the version of Istio that you installed.
  ```
  istioctl version
  ```
  {: pre}

2. Download the version of `istioctl` that matches your cluster's Istio version.
  ```
  curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.6.8 sh -
  ```
  {: pre}
3. Navigate to the Istio package directory.
  ```
  cd istio-1.6.8
  ```
  {: pre}
4. MacOS and Linux users: Add the `istioctl` client to your `PATH` system variable.
  ```
  export PATH=$PWD/bin:$PATH
  ```
  {: pre}

<br />


## Customizing the Istio installation
{: #customize}

In version 1.5 and later of the Istio add-on, you can customize a set of Istio configuration options by editing the `managed-istio-custom` configmap resource. These settings include extra control over monitoring, logging, and networking in your control plane and service mesh.
{: shortdesc}

1. Edit the `managed-istio-custom` configmap resource.
  ```
  kubectl edit cm managed-istio-custom -n ibm-operators
  ```
  {: pre}

2. Add a `data` section, and add the `<key>: "<value>"` pair of one or more of the following configuration options.
    <table summary="The rows are read from left to right. The first column is the configuration option name. The second column is the default value of the option. The third column contains a brief description of the option.">
    <caption>Istio add-on configuration options</caption>
    <thead>
    <th>Configuration option</th>
    <th>Default value</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr><td>`istio-global-logging-level`</td><td>`"default:info"`</td><td>Define the scope of logs and the level of log messages for control plane components. A scope represents a functional area within a control plane component and each scope supports specific log information levels. The `default` logging scope, which is for non-categorized log messages, is applied to all components in the control plane at the basic `info` level.</br></br>To specify log levels for individual component scopes, enter a comma-separated list of scopes and levels, such as `"<scope>:<level>,<scope>:<level>"`. For a list of the scopes for each control plane component and the information level of log messages, see the [Istio component logging documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/latest/docs/ops/diagnostic-tools/component-logging/).<p class="tip">To change the log level of the data plane, use the `istioctl proxy-config log <pod> --level <level>` command.</p></td></tr>
    <tr><td>`istio-global-outboundTrafficPolicy-mode`</td><td>`"ALLOW_ANY"`</td><td>By default, all outbound traffic from the service mesh is permitted. To block outbound traffic from the service mesh to any host that is not defined in the service registry or that does not have a `ServiceEntry` within the service mesh, set to `REGISTRY_ONLY`.</td></tr>
    <tr><td>`istio-global-proxy-accessLogFile`</td><td>""</td><td>Envoy proxies print access information to their standard output. To view this access information when running `kubectl logs` commands for the Envoy containers, set to `"/dev/stdout"`.</td></tr>
    <tr><td>**Beta**: `istio-ingressgateway-public-1|2|3-enabled`</td><td>`"true"` in zone 1 only</td><td>To make your apps more highly available, set to `"true"` for each zone where you want a public `istio-ingressgateway` load balancer to be created.</td></tr>
    <tr><td>**Beta**: `istio-ingressgateway-zone-1|2|3`</td><td>`"<zone>"`</td><td>The zones where your worker nodes are deployed. These fields apply your cluster's zones to the `istio-ingressgateway-public-1|2|3-enabled` fields.<ul><li>If you installed the Istio add-on at version 1.5 or later, your cluster's zones are already listed.</li><li>If you updated your Istio add-on from version 1.4 to 1.5, you must add your cluster's zones.</li></ul></td></tr>
    <tr><td>`istio-kiali-dashboard-viewOnlyMode`</td><td>`"true"`</td><td>No changes can be made to the Kiali dashboard in view-only mode by default. To allow changes in view-only mode, set to `false`.</td></tr>
    <tr><td>1.6 only: `istio-knative-cluster-local-gateway-enabled`</td><td>`"false"`</td><td>Automatically incorporate Knative apps into the Istio service mesh by enabling the Knative cluster local gateway. When version 0.15.1 of the Knative add-on is enabled, the value for this key is set to `"true"` by default. Note that only Knative version 0.15.1 is supported for Istio version 1.6, and that if you installed an older version of Knative, you must manually update your Knative add-on. For more information, see [Deploying serverless apps with Knative (beta)](/docs/containers?topic=containers-serverless-apps-knative).</td></tr>
    <tr><td>`istio-monitoring`</td><td>`"false"`</td><td>The Prometheus, Grafana, Jaeger, and Kiali monitoring components are disabled by default due to current security concerns in the community release of Istio that are not adequately addressed for a production environment. To enable these monitoring components, set to `true`.<p class="tip">For further health insights, your Istio installation is preconfigured for you to use [{{site.data.keyword.mon_full_notm}}](/docs/containers?topic=containers-istio-health#istio_health_sysdig) to monitor your Istio-managed apps.</p><p class="note">All `istio-monitoring` support might be removed in a later version of the managed Istio add-on. To use monitoring with Istio, you must install the components separately from the Istio add-on. For more information, see the [Istio documentation](https://istio.io/latest/docs/ops/integrations/){: external}.</p></td></tr>
    <tr><td>1.6 only: `istio-monitoring-telemetry`</td><td>`"true"`</td><td>By default, telemetry metrics and Prometheus support is enabled. To remove any performance issues associated with telemetry metrics and disable all monitoring, set to `"false"`.</p></td></tr>
    <tr><td>`istio-pilot-traceSampling`</td><td>`"1.0"`</td><td>By default, Istio [generates trace spans for 1 out of every 100 requests ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/latest/docs/tasks/observability/distributed-tracing/overview/#trace-sampling), which is a sampling rate of 1%. To generate more trace spans, increase the percentage value.</td></tr>
    </tbody>
    </table>

3. Save and close the configuration file.

4. Apply the changes to your Istio installation. Changes might take up to 10 minutes to take effect.
  ```
  kubectl annotate iop -n ibm-operators managed-istio --overwrite version="custom-applied-at: $(date)"
  ```
  {: pre}

5. If you changed the `istio-global-logging-level` or `istio-global-proxy-accessLogFile` settings, you must restart your data plane pods to apply the changes to them.
  1. Get the list of all data plane pods that are not in the `istio-system` namespace.
    ```
    istioctl version --short=false | grep "data plane version" | grep -v istio-system
    ```
    {: pre}

    Example output:
    ```
    data plane version: version.ProxyInfo{ID:"test-6f86fc4677-vsbsf.default", IstioVersion:"1.5"}
    data plane version: version.ProxyInfo{ID:"rerun-xfs-f8958bb94-j6n89.default", IstioVersion:"1.5"}
    data plane version: version.ProxyInfo{ID:"test2-5cbc75859c-jh6bx.default", IstioVersion:"1.5"}
    data plane version: version.ProxyInfo{ID:"minio-test-78b5d4597d-hkpvt.default", IstioVersion:"1.5"}
    data plane version: version.ProxyInfo{ID:"sb-887f89d7d-7s8ts.default", IstioVersion:"1.5"}
    data plane version: version.ProxyInfo{ID:"gid-deployment-5dc86db4c4-kdshs.default", IstioVersion:"1.5"}
    ```
    {: screen}
  2. Restart each pod by deleting it. In the output of the previous step, the pod name and namespace are listed in each entry as `data plane version: version.ProxyInfo{ID:"<pod_name>.<namespace>", IstioVersion:"1.5"}`.
    ```
    kubectl delete pod <pod_name> -n <namespace>
    ```
    {: pre}

<p class="tip">If you later want to change a setting that you added to the configmap, you can use a patch script. For example, if you added the `istio-monitoring: "true"` setting and later want to change it back to `"false"`, you can run `kubectl patch cm managed-istio-custom -n ibm-operators --type='json' -p='[{"op": "add", "path": "/data/istio-monitoring", "value":"false"}]'`. Then, apply your changes by running `kubectl annotate iop -n ibm-operators managed-istio --overwrite version="custom-applied-at: $(date)"`.</p>

<p class="note">If you disable the Istio add-on, the `managed-istio-custom` configmap is not removed during uninstallation. When you re-enable the Istio add-on, your customized configmap is applied during installation.</br></br>If you do not want to re-use your custom settings in a later installation of Istio, you must delete the configmap after you disable the Istio add-on by running `kubectl delete cm -n ibm-operators managed-istio-custom`. When you re-enable the Istio add-on, the default configmap is applied during installation.</p>

<br />


## Updating the Istio add-on
{: #istio_update}

Update your Istio add-on to the latest version, which is tested by {{site.data.keyword.cloud_notm}} and approved for the use in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Do not use `istioctl` to update the version of Istio that is installed by the managed add-on. Only use the following steps to update your managed Istio add-on, which includes an update of the Istio version.
{: important}

### Updating the minor version of the Istio add-on
{: #istio_minor}

{{site.data.keyword.cloud_notm}} keeps all your Istio components up-to-date by automatically rolling out patch updates to the most recent version of Istio that is supported by {{site.data.keyword.containerlong_notm}}. To update your Istio components to the most recent minor version of Istio that is supported by {{site.data.keyword.containerlong_notm}}, such as from version 1.4 to 1.5, you must manually update your add-on.
{: shortdesc}

When you update the Istio control components in the `istio-system` namespace to the latest minor version, you might experience disruptive changes. Review the following changes that occur during a minor version update.
* As updates are rolled out to control plane pods, the pods are re-created. The Istio control plane is not fully available until after the update completes.
* The Istio data plane continues to function during the update. However, some traffic to apps in the service mesh might be interrupted for a short period of time.
* The external IP address for the `istio-ingressgateway` load balancer does not change during or after the update.

If your Istio add-on runs version 1.3 or earlier, you must update your add-on by following the steps in [Updating your add-on from beta versions to the generally available version](#istio-ga).
{: note}

You cannot revert your managed Istio add-on to a previous version. If you want to revert to an earlier minor version, such as from version 1.5 to 1.4, you must uninstall your add-on and then reinstall the add-on by specifying the earlier version.
{: important}

1. Review the available Istio add-on versions.
  ```
  ibmcloud ks addon-versions
  ```
  {: pre}

2. Review the changes that are included in each version in the [Istio add-on changelog](/docs/containers?topic=containers-istio-changelog).

3. Update the Istio add-on.
  ```
  ibmcloud ks cluster addon update istio --version <version> -c <cluster_name_or_ID>
  ```
  {: pre}

4. Before you proceed, verify that the update is complete. <p class="note">The update process can take up to 20 minutes to complete.</p>
  1. Ensure that the Istio add-on's **Health State** is `normal` and the **Health Status** is `Addon Ready`. If the state is `updating`, the update is not yet complete.
    ```
    ibmcloud ks cluster addon ls -c <cluster_name_or_ID>
    ```
    {: pre}
  2. Ensure that the control plane component pods in the `istio-system` namespace have a **STATUS** of `Running`.
      ```
      kubectl get pods -n istio-system
      ```
      {: pre}

      ```
      NAME                                                     READY   STATUS    RESTARTS   AGE
      istio-system    istio-egressgateway-6d4667f999-gjh94     1/1     Running     0          61m
      istio-system    istio-egressgateway-6d4667f999-txh56     1/1     Running     0          61m
      istio-system    istio-ingressgateway-7bbf8d885-b9xgp     1/1     Running     0          61m
      istio-system    istio-ingressgateway-7bbf8d885-xhkv6     1/1     Running     0          61m
      istio-system    istiod-5b9b5bfbb7-jvcjz                  1/1     Running     0          60m
      istio-system    istiod-5b9b5bfbb7-khcht                  1/1     Running     0          60m
      ```
      {: screen}

5. [Update your `istioctl` client and sidecars](#update_client_sidecar).

6. **Version 1.5 and 1.6**: If you update your Istio version to 1.5 or 1.6, review the following changes that are required for monitoring setups in your service mesh.
    * In versions 1.5 and 1.6, the Prometheus, Grafana, Jaeger, and Kiali monitoring components are included in your Istio installation but are **disabled** by default. To securely enable these monitoring components, see [Enabling Prometheus, Grafana, Jaeger, and Kiali](/docs/containers?topic=containers-istio-health#enable_optional_monitor).
    * In version 1.5 only, if you use Sysdig to monitor your Istio-managed apps, [update the `sysdig-agent` configmap so that sidecar metrics are tracked](/docs/containers?topic=containers-istio-health#sysdig-15).

### Updating your add-on from beta versions to the generally available version
{: #istio-ga}

The Istio managed add-on is generally available for Kubernetes version 1.16 and later clusters as of 19 November 2019. The beta version of the managed add-on, which runs Istio version 1.3 or earlier, can no longer be installed on 12 February 2020. In Kubernetes version 1.16 or later clusters, you can [update your add-on](#istio_update) by uninstalling the Istio version 1.3 or earlier add-on and installing the Istio version 1.4 or later add-on.
{: shortdesc}

After you install the Istio version 1.4 or later add-on in a Kubernetes version 1.16 or later cluster, {{site.data.keyword.cloud_notm}} keeps all your Istio components up-to-date by automatically rolling out patch updates. To update your Istio components to the most recent minor version of Istio that is supported by {{site.data.keyword.containerlong_notm}}, such as from version 1.4 to 1.5, you can follow the steps in [Updating the minor version of the Istio add-on](#istio_minor). You can see the changes that are applied in each update in the [Istio add-on changelog](/docs/containers?topic=containers-istio-changelog).

1. Save any resources, such as configuration files for any services or apps, that you created or modified in the `istio-system` namespace. Example command:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

2. Save the Kubernetes resources that were automatically generated in the namespace of the managed add-on to a YAML file on your local machine. These resources are generated by using custom resource definitions (CRDs).
   1. Get the CRDs for your add-on.
      ```
      kubectl get crd
      ```
      {: pre}

   2. Save any resources created from these CRDs.

3. If you enabled the `istio-sample-bookinfo` and `istio-extras` add-ons, disable them.
   1. Disable the `istio-sample-bookinfo` add-on.
      ```
      ibmcloud ks cluster addon disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. Disable the `istio-extras` add-on.
      ```
      ibmcloud ks cluster addon disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

4. Disable the Istio add-on.
   ```
   ibmcloud ks cluster addon disable istio --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

5. Before you continue to the next step, verify that all add-on resources are removed.
  1. Verify that the beta Istio add-ons are removed.
     ```
     ibmcloud ks cluster addon ls --cluster <cluster_name_or_ID>
     ```
     {: pre}
  2. Verify that the `istio-system` namespace is removed.
    ```
    kubectl get namespaces
    ```
    {: pre}

6. Re-enable Istio. The default version of the generally available Istio managed add-on, 1.4, is installed.
   ```
   ibmcloud ks cluster addon enable istio --cluster <cluster_name_or_ID>
   ```
   {: pre}

7. Apply the CRD resources that you saved in step 2.
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

8. Optional: [Install the BookInfo sample app.](/docs/containers?topic=containers-istio-mesh#bookinfo_setup) In version 1.4 or later of the managed Istio add-on, the Grafana, Jaeger, and Kiali components that were previously installed by the `istio-extras` add-on are now included in the `istio` add-on. However, the BookInfo sample app that was previously installed by the `istio-sample-bookinfo` add-on is not included in the `istio` add-on and must be installed separately.

9. Optional: If you use TLS sections in your gateway configuration files, you must delete and re-create the gateways so that Envoy can access the secrets.
  ```
  kubectl delete gateway mygateway
  kubectl apply -f mygateway.yaml
  ```
  {: pre}

10. Next, [update your `istioctl` client and sidecars](#update_client_sidecar).

### Updating the `istioctl` client and sidecars
{: #update_client_sidecar}

Whenever the Istio managed add-on is updated, update your `istioctl` client and the Istio sidecars for your app.
{: shortdesc}

For example, the patch version of your add-on might be updated automatically by {{site.data.keyword.containerlong_notm}}, or you might [update the minor version of your add-on](#istio_minor). In either case, update your `istioctl` client and your app's existing Istio sidecars to match the Istio version of the add-on.

1. Get the version of your `istioctl` client and the Istio add-on control plane components.
  ```
  istioctl version --short=false
  ```
  {: pre}
  Example output:
  ```
  client version: 1.5.7
  cluster-local-gateway version:
  citadel version: 1.6.8
  egressgateway version: 1.6.8
  egressgateway version: 1.6.8
  galley version: 1.6.8
  ingressgateway version: 1.6.8
  ingressgateway version: 1.6.8
  pilot version: 1.6.8
  policy version: 1.6.8
  sidecar-injector version: 1.5.7
  telemetry version: 1.6.8
  data plane version: version.ProxyInfo{ID:"cluster-local-gateway-859958cb-fjv2d.istio-system", IstioVersion:"1.5.7"}
  data plane version: version.ProxyInfo{ID:"istio-egressgateway-7966998fd7-vxhm6.istio-system", IstioVersion:"1.5.7"}
  data plane version: version.ProxyInfo{ID:"webserver-6c6db9ffbc-xzjzl.default", IstioVersion:"1.5.7"}
  ...
  ```
  {: screen}

2. In the output, compare the `client version` (`istioctl`) to the version of the Istio control plane components, such as the `pilot version`. If the `client version` and control plane component versions do not match:
    1. Download the `istioctl` client of the same version as the control plane components.
      ```
      curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.6.8 sh -
      ```
      {: pre}
    2. Navigate to the Istio package directory.
      ```
      cd istio-1.6.8
      ```
      {: pre}
    3. MacOS and Linux users: Add the `istioctl` client to your `PATH` system variable.
      ```
      export PATH=$PWD/bin:$PATH
      ```
      {: pre}

3. In the output of step 1, compare the `pilot version` to the `data plane version` for each data plane pod.
  * If the `pilot version` and the `data plane version` match, no further updates are required.
  * If the `pilot version` and the `data plane version` do not match, restart your deployments for the data plane pods that run the old version. The pod name and namespace are listed in each entry as `data plane version: version.ProxyInfo{ID:"<pod_name>.<namespace>", IstioVersion:"1.5"}`.
    ```
    kubectl rollout restart deployment <deployment> -n <namespace>
    ```
    {: pre}

<br />


## Uninstalling Istio
{: #istio_uninstall}

If you're finished working with Istio, you can clean up the Istio resources in your cluster by uninstalling the Istio add-ons.
{: shortdesc}

The `istio` add-on is a dependency for the [`knative`](/docs/containers?topic=containers-serverless-apps-knative) add-on.
{: important}

### Managing resources before uninstallation
{: #uninstall_resources}

Review the following optional steps for saving or deleting custom Istio resources before you uninstall Istio.
{: shortdesc}

1. Any resources that you created or modified in the `istio-system` namespace and all Kubernetes resources that were automatically generated by custom resource definitions (CRDs) are removed. If you want to keep these resources, save them before you uninstall the Istio add-on.
  1. Save any resources, such as configuration files for any services or apps, that you created or modified in the `istio-system` namespace.
     Example command:
     ```
     kubectl get pod <pod_name> -o yaml -n istio-system
     ```
     {: pre}
  2. Get the CRDs in `istio-system`.
    ```
    kubectl get crd -n istio-system
    ```
    {: pre}
  3. Save the Kubernetes resources that were automatically generated by these CRDs to a YAML file on your local machine.

2. The `managed-istio-custom` configmap is not removed during uninstallation. If you later re-enable the Istio add-on, any [customized settings that you made to the configmap](#customize) are applied during installation. If you do not want to re-use your custom settings in a later installation of Istio, you must delete the configmap.
  ```
  kubectl delete cm -n ibm-operators managed-istio-custom
  ```
  {: pre}

</br>

### Uninstalling the Istio add-on from the console
{: #istio_uninstall_ui}

1. In your [cluster dashboard](https://cloud.ibm.com/kubernetes/clusters){: external}, click the name of the cluster where you want to remove the Istio add-on.

2. Click the **Add-ons** tab.

3. On the Managed Istio card, click the Action menu icon.

4. Click **Uninstall**. The managed Istio add-on is disabled in this cluster and all Istio resources in this cluster are removed.

5. On the Managed Istio card, verify that the add-on you uninstalled is no longer listed.

</br>
### Uninstalling managed Istio add-ons from the CLI
{: #istio_uninstall_cli}

If you did not install the deprecated `istio-sample-bookinfo` and `istio-extras` add-ons, skip steps 1 and 2.
{: tip}

1. Disable the `istio-sample-bookinfo` add-on.
  ```
  ibmcloud ks cluster addon disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Disable the `istio-extras` add-on.
  ```
  ibmcloud ks cluster addon disable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Disable the `istio` add-on.
  ```
  ibmcloud ks cluster addon disable istio --cluster <cluster_name_or_ID> -f
  ```
  {: pre}

4. Verify that all managed Istio add-ons are disabled in this cluster. No Istio add-ons are returned in the output.
  ```
  ibmcloud ks cluster addon ls --cluster <cluster_name_or_ID>
  ```
  {: pre}

</br>

### Uninstalling other Istio installations in your cluster
{: #istio_uninstall_other}

If you previously installed Istio in the cluster by using the IBM Helm chart or through another method, clean up that Istio installation before you enable the managed Istio add-on in the cluster. To check whether Istio is already in a cluster, run `kubectl get namespaces` and look for the `istio-system` namespace in the output.
{: shortdesc}

- If you installed Istio by using the {{site.data.keyword.cloud_notm}} Istio Helm chart:
  1. Uninstall the Istio Helm deployment.
    ```
    helm del istio --purge
    ```
    {: pre}

  2. If you used Helm 2.9 or earlier, delete the extra job resource.
    ```
    kubectl -n istio-system delete job --all
    ```
    {: pre}

  3. The uninstallation process can take up to 10 minutes. Before you install the Istio managed add-on in the cluster, run `kubectl get namespaces` and verify that the `istio-system` namespace is removed.

- If you installed Istio manually or used the Istio community Helm chart, see the [Istio uninstall documentation](https://istio.io/latest/docs/setup/getting-started/#uninstall){: external}.
* If you previously installed BookInfo in the cluster, clean up those resources.
  1. Change the directory to the Istio file location.
    ```
    cd <filepath>/istio-1.6.8
    ```
    {: pre}

  2. Delete all BookInfo services, pods, and deployments in the cluster.
    ```
    samples/bookinfo/platform/kube/cleanup.sh
    ```
    {: pre}

  3. The uninstallation process can take up to 10 minutes. Before you install the Istio managed add-on in the cluster, run `kubectl get namespaces` and verify that the `istio-system` namespace is removed.

<br />


## Troubleshooting
{: #istio-ts}

To resolve some common issues that you might encounter when you use the managed Istio add-on, see [Troubleshooting managed add-ons](/docs/containers?topic=containers-cs_troubleshoot_addons).
{: shortdesc}
