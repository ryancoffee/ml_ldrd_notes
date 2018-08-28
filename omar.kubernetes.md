## Kubernetes for Benchmarking Models on differnt acceleration hardware

You can use Kubernetes to where you can specify the gpu label and even control the memory on the GPU so that we can distribute to the Volta and the P40 with seperate kubernetes instances and benchmark the two.
https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/
https://docs.nvidia.com/datacenter/kubernetes-install-guide/index.html

