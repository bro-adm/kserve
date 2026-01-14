## Basics

Understanding VLLM concepts (prefil, batch, decode)
Understanding Gateways (relevant for MAAS as well via Kuadrant)

## Gateway & Indepedent Scallability

[GAIE Architecture](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/inference-gateway-architecture.svg)

[GAIE EPP (Inference Scheduler)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/endpoint-picker.svg)

[GAIE Project & README](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main)

From this we can understand that the GAIE EPP (inference shceduler implementation) is logical routing only (picker), it does not route anything itself.

[Understanding Proxy PD Disaggregation Concept](https://github.com/vllm-project/vllm/blob/main/docs/design/p2p_nccl_connector.md)

[Understandning Proxy PD Routing Flow](https://github.com/opendatahub-io/llm-d-routing-sidecar)

I think its safe to say that at this point we can assume that the routing sidecar is deployed per set of pd pods.
I guess it can also run as the sidecar of the decode pod, becuase it first passes the request to the prefill worker.

EPP can laod plugins like supporting PD Disaggregation and if reuired i guess it acknoledges the API for the router sidecar, and probably chooses the model via the decode pod referenced under the InfrencePool as an InferenceModel.

[LLM-D Inference Scheduler Fork](https://github.com/opendatahub-io/llm-d-inference-scheduler/blob/main/docs/architecture.md)

The fork exisst only as a time saver/unblocker and is intended to be fully encapsulated into the GAIE as the EPP implementation.
I also think this acknoledges what was said up until now.
I think its also now answered how the EPP as a non router/proxier passes the routing decision back to the GAIE on the Gateway in a InferencePool native design.

[How Does the Gateway Work Part 1](https://docs.google.com/document/d/1Lqh9GA-yfFl-thFuvahuwcqouMGnHJCyjii0Vv-677g/edit?tab=t.0)

[How Does the Gateway Work Part 2](https://ovn-kubernetes.io/)

[How does the Gateway Work Part 3](https://gateway-api.sigs.k8s.io/#whats-the-difference-between-gateway-api-and-an-api-gateway)

I nice thing to notice is that the default gateway is deployed under `openshift-ingress` namepsace.
Of course you can deploy a custom/non-default gateway for a set of workloads.

[LLMISVC](https://github.com/red-hat-data-services/kserve/tree/main/pkg/controller/llmisvc)

[LLMISVC PodMonitor & ServiceMonitor Generator (and a bit more)](github.com/red-hat-data-services/kserve/blob/rhoai-3.0/pkg/controller/llmisvc/monitoring.go)

[LLMISVC Specs](https://github.com/red-hat-data-services/kserve/blob/main/pkg/apis/serving/v1alpha1/llm_inference_service_types.go)

[LLMISVC Samples](https://github.com/red-hat-data-services/kserve/tree/rhoai-3.0/docs/samples/llmisvc)

You will find that the inference shceduler (the EPP) will be monitored via a ServiceMonitor and the inference server (vLLM) will be monitored via PodMonitor (makes sense)

> In general ODH is seen as the Midstream step where all the forks reside.
> The Downstream repo itself is irrelevant for now.
> The Upstreams are forked multiple times as stated above.
> Some ODH projects if not all are also forked for some reason ([link](github.com/red-hat-data-services))
> Another interesting git is the [bu](https://github.com/rh-aiservices-bu) repo. Git based guides for example [RHAIIS](https://github.com/rh-aiservices-bu/rhaiis-demo) (rhaiis will kepp coming up)
> Another one is the [Emerging Technologies](https://github.com/redhat-et)

[Rhoai Monitored Components](https://github.com/opendatahub-io/opendatahub-operator/blob/main/config/monitoring/prometheus/apps/prometheus-configs.yaml)
Todo -> Make monirotng rules be per component and not centralized.

- Missing Support Matrix for projects and dependcies on diff infras
- Still missing explanation for myself about the EPP CRD
