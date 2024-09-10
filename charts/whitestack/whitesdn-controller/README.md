# WhiteSdn-controller

This chart install `whitesdn-controlller`, a Kubernetes controller that manages
the lifecycle of networks and subnets in a WhiteSdn cluster for baremetal worker
nodes that use secondary pod interfaces such as SR-IOV and VLAN.

## Configuration

You need the following information to install this controller:

- WhiteSdn API URL
- WhiteSdn API User Credentials
- A random availability zone, must start with the `WCRUISER_` prefix
- List of nodes and connections to switches managed by WhiteSdn in JSON format,
  for example:

  ```json
  {
    "nodes": [
      {
        # Name of the worker node, must be the same as the registered in
        # Kubernetes
        "name": "worker-node-1",
        # List of switch/interface that have a connection to this node
        "ports": [
          {
            "device_id": "switch1",
            # This interface must be a po/sa (for OCNOS switches)
            "interface_name": "po171"
          },
          {
            "device_id": "switch2",
            "interface_name": "po171"
          }
        ]
      }
    ]
  }
  ```

## Verify Installation

This chart will install the controller as a Deployment in the defined namespace.
To check if it's installed correctly you can run the following command:

```shell
kubectl -n whitesdn-controller get pods

NAME                                           READY   STATUS    RESTARTS   AGE
whitesdn-controller-manager-6c7865874f-n96hf   2/2     Running   0          43s
```

And retrieve its logs by running:

```shell
kubectl -n whitesdn-controller logs whitesdn-controller-manager-6c7865874f-n96hf

2024-07-19T22:08:33Z INFO setup   starting manager
2024-07-19T22:08:33Z INFO starting server {"kind": "health probe", "addr": "[::]:8081"}
...
```

## Usage

This controller runs everytime a NAD resource is created or modified and
periodically after that so user interaction is not required. This controller
will configure the Kubernetes nodes that have the SR-IOV operator label
`feature.node.kubernetes.io/network-sriov.capable=true` or the custom label
`whitestack.com/whitesdn-controller=true`, so if your cluster is not using the
SR-IOV operator please set the custom label in the required nodes.

You can verify if a NAD has been processed correctly by checking if a finalizer
is present in the manifest named `whitesdn.whitestack.com/finalizer`. If the
finalizer is not present you should check if there are any errors in the
controller's logs.
