## Energy-Efficient Pod Scheduling and Load Balancing with the Workload Allocation Optimizer (WAO)

Modern datacenters consume enormous amounts of electricity, a challenge accelerated by the growth of AI workloads.
**WAO** offers a **purely software-based approach** to reduce server energy consumption by predicting power usage on individual nodes and optimizing workload placement.

We integrated WAO with Kubernetes in two ways:
- [**WAO-Scheduler**](https://github.com/waok8s/wao-scheduler) uses a custom scheduler plugin to assign Pods to nodes likely to incur the smallest increase in power usage.
- [**WAO-LoadBalancer**](https://github.com/waok8s/wao-loadbalancer) is a customized kube-proxy that balances traffic across nodes to minimize power usage from incoming requests.

Both of these components rely on the WAO predictive model:
- It forecasts server power consumption using CPU usage, inlet temperature, and static pressure as inputs.
- A separate model must be developed for each server type through measurement, yet remains independent of the server’s physical deployment environment.

We conducted a series of experiments in a real-world datacenter and found:
- **WAO-Scheduler** achieved up to a 10–20% reduction in overall power consumption in real-world tests.
- **WAO-LoadBalancer** is still under active development.

This open-source implementation includes [custom resources](https://github.com/waok8s/wao-core) for per-node configuration, a [metrics adapter](https://github.com/waok8s/wao-metrics-adapter) to collect environmental data, and integrations with both the scheduler plugin and our custom kube-proxy.

Future work involves integrating with cooling systems, enhancing workload rebalancing, and extending support to GPUs. By leveraging Kubernetes extensibility and predictive modeling, we aim to make datacenters more energy-efficient without compromising user experience.

### Additional Resources

- Presentation at Kubernetes Meetup Tokyo #66 (日本語): [[Video]](https://www.youtube.com/live/RpaC3AG2bc4?t=2490s) [[Slides]](https://speakerdeck.com/ebiiim/waok8s-k8sjp66)
- Experiments in datacenter: [Ying-Feng Hsu et al., "Sustainable Data Center Energy Management through Server Workload Allocation Optimization and HVAC System", IEEE Cloud Summit 2024, Washington DC, USA, 2024.](https://ieeexplore.ieee.org/document/10630908)
- WAO: [R. Douhara et al., "Kubernetes-based Workload Allocation Optimizer for Minimizing Power Consumption of Computing System with Neural Network," 2020 International Conference on Computational Science and Computational Intelligence (CSCI), Las Vegas, NV, USA, 2020.](https://ieeexplore.ieee.org/document/9458062)