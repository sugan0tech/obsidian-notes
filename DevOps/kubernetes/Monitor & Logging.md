- no built in options from kubernetest


- Varieties of 3rd party tools does the job
	- heapster ( the originial tool made for k8s , depricated & replaced by light weight version ==metric server==)
	- metric server ( in memory )
	- prometheous
	- datadog

### How metrics collected
kubelet -> Cadvisor -> get metrics from pods



### how to setup - metric server
- for minikube just via plugin command
`minikube addons enable metric-server`
- anything else, need to pull up repo and apply it's deployment configurations


to get these, run `kubectl top node` - to get node level metrics


## Monitoring Logs