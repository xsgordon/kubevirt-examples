kubevirt-examples
=================

Overview
--------

This repository contains YAML files to facilitate performing a simple
demonstration of the [KubeVirt](http://kubevirt.io/) project and related
components including the [Containerized Data Importer][1].

Pre-requisites
--------------

It is expected that these will be run against a Kubernetes or OpenShift
environment with KubeVirt and the Containerized Data Importer installed and
configured.

Inventory
---------

The following examples use the Cirros disk image and are found in
`./cirros/`:

* `cirros-vm.yaml` - Create a basic Cirros virtual machine using a
  `RegistryDisk`, pulls image from Docker Hub.
* `cirros-pvc.yaml` - Create a `PersistentVolume` containing a Cirros disk image
  using the Containerized Data Importer, `img` file pulled via HTTP.
* `cirros-pvc-vm.yaml` - Create a basic Cirros virtual machine using a
  `PersistentVolume`.
* `cirros-clone-pvc.yaml` - Create a clone of the `PersistentVolume` created by
  `cirros-pvc.yaml` using the Containerized Data Importer.
* `cirros-clone-vm.yaml` - Create a virtual machine based on the cloned
  `PersistentVolume` from `cirros-clone-pvc.yaml`.

The `fedora-*` examples in `./examples/fedora/` follow the same pattern as the
Cirros examples above. In both the Cirros and Fedora examples the resultant
virtual machine has basic networking provided by the `Pod` network. The Cirros
image uses the default username and password baked into it (`cirros` and
`gocubsgo`), the Fedora image uses the `shadowman` user with the `shadowman`
password.

Basic KubeVirt Demo Flow
------------------------

Once the environment is operational, it is ready for demonstration. A typical
basic flow looks something like this:

1. Examine the `kube-system` namespace illustrating the presence of the KubeVirt
   components.
2. Use `kubectl create -f examples/cirros/cirros-vm.yaml` to create a basic
   Cirros virtual machine based on a `RegistryDisk`.
3. Use `kubectl get vms` to illustrate `kubectl` can see the vm object.
4. Use `kubectl edit vm cirros-vm` or `virtctl start cirros-vm` to create the
   `VirtualMachineInstance`.
5. Use `kubectl get vmis` to illustrate the presence of the VM instance.
6. Use `kubectl describe vmi cirros-vm` to see the scheduling state of the
   virtual machine instance.
7. Use `virtctl console cirros-vm` to access the console of the virtual machine
   instance.

Basic KubeVirt + Containerized Data Importer Demo Flow
------------------------------------------------------

The Containerized Data Importer allows us to demonstrate spawning a virtual
machine that uses a `PersistentVolume` instead of a `RegistryDisk`.

1. Use `kubectl create -f examples/cirros/cirros-pvc.yaml` to create the Cirros
   `PersistentVolume`.
2. Use `watch -n 2 kubectl logs <pod>` to illustrate the state of the import
   process.
3. Use `kubectl create -f examples/cirroscirros-pvc-vm.yaml` to create a basic
   Cirros virtual machine based on a `PersistentVolume`.
4. Use `kubectl create -f examples/cirroscirros-clone-pvc.yaml` to demonstrate
   cloning a volume.
5. Use `kubectl create -f examples/cirros/cirros-clone-vm.yaml` to demonstrate
   spawning a virtual machine from the cloned volume.

[1]: http://github.com/kubevirt/containerized-data-importer
