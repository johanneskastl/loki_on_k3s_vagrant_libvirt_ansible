# loki_on_k3s_vagrant_libvirt_ansible

Vagrant-libvirt setup that creates a VM with a [k3s](k3s.io) Kubernetes cluster.
In the cluster, [Grafana Loki](https://grafana.com/docs/loki/latest/) is
installed, together with Grafana (the graphical dashboard) and
[Promtail](https://grafana.com/docs/loki/latest/send-data/promtail/).

There is a second branch called **Vector** where [Vector](https://vector.dev/)
is used instead of Promtail.

There is a third branch called **GrafanaAgent** where Grafana Agent is used
instead of Promtail.

There is a fourth branch called **GrafanaAgent_via_Operator** where Grafana
Agent is used instead of Promtail. This time it is installed using the
[operator](https://grafana.com/docs/agent/latest/operator/getting-started/).

There is a fifth branch called **GrafanaAgentFlowMode** where Grafana Agent is
used instead of Promtail, but this time in [flow
mode](https://grafana.com/docs/agent/latest/flow/).

There is a sixth branch called **Alloy** where [Grafana
Alloy](https://github.com/grafana/alloy) is used
instead of Promtail.

Default OS is openSUSE Leap 15.5, but that can be changed in the Vagrantfile.
Please be aware, that this might break the Ansible provisioning.

## Vagrant

1. You need `vagrant`, obviously. And `git`. And Ansible...
1. Fetch the box, per default this is `opensuse/Leap-15.5.x86_64`, using
   `vagrant box add opensuse/Leap-15.5.x86_64`.
1. Make sure the git submodules are fully working by issuing
   `git submodule init && git submodule update`
1. Run `vagrant up`
1. Open the Grafana URL that Ansible printed out at the end of the provisioning.
   Use the admin password that Ansible also printed out.
1. Go to `Explore` on the left-hand side menu and start roaming around the Loki
   data source. Select the `component` label, select a component and click `Run
   the query`.
1. Party!

## Disabling the Ansible provisioning

In case you do not want Ansible to install k3s (because you want to install it
yourself), just comment out the following lines in the `Vagrantfile`:

```
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision
```

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This will
also remove the kubeconfig file `ansible/k3s-kubeconfig`.
