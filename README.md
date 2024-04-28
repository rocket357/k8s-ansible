## k8s-ansible
This is a series of playbooks that allow the user to keep an existing on-prem kubernetes
installation up-to-date.  It's written with Debian in mind (tested on Debian 12, currently)
and assumes the hosts are running in Xen.  The playbooks are broken down by topic, and anything
beginning with 0#-kubernetes only touches the kubernetes installation (i.e. only the 01-debian.yml
playbook does anything host-related).

## Playbooks
The playbooks are broken down by topic.  02-kubernetes.yml performs a "latest and greatest" version
check against https://kubernetes.io/releases/#release-history and configures the apt sources to point 
to the latest and greatest version (v1.30 at the time of writing).  It then performs a host-by-host
control plane update/kubelet restart in a controlled fashion, then upgrades the CNI (calico with metallb, 
adjust to suite your needs), then performs a host-by-host update/kubelet restart of the workers.  Once 
that is done, the CSI(s) are upgraded (OpenEBS), infrastructure bits are upgraded (cert-manager, ingress-nginx,
vault), operators (strimzi kafka operator and zalando postgres-operator), operator crs (kafka cluster and redis),
and finally, applications are installed (uptime-kuma, wbo and changedetection-io).

These are just my preferences, feel free to add/remove as you see fit!