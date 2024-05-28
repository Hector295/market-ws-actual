# WhiteSdn-controller

This chart install `whitesdn-controlller`, a Kubernetes controller that manages
the lifecycle of networks and subnets in a WhiteSdn cluster for baremetal worker
nodes that use secondary pod interfaces such as SR-IOV, IPVlan and MacVlan.

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
