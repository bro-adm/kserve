# LLMInferenceService CRD Comparison: Downstream vs Upstream

This document provides a comprehensive comparison of the LLMInferenceService and LLMInferenceServiceConfig Custom Resource Definitions (CRDs) between the downstream-view and upstream-view of the KServe repository.

## Comparison Date
Generated: 2026-01-14

## Source Files Analyzed

### Downstream View
- `downstream-view/config/crd/full/serving.kserve.io_llminferenceservices.yaml`
- `downstream-view/config/crd/full/serving.kserve.io_llminferenceserviceconfigs.yaml`
- Sample YAMLs in `downstream-view/docs/samples/llmisvc/`

### Upstream View
- `upstream-view/config/crd/full/llmisvc/serving.kserve.io_llminferenceservices.yaml`
- `upstream-view/config/crd/full/llmisvc/serving.kserve.io_llminferenceserviceconfigs.yaml`
- Sample YAMLs in `upstream-view/docs/samples/llmisvc/`

---

## Summary Comparison Table

| Aspect | Downstream | Upstream | Status |
|--------|-----------|----------|--------|
| **API Version** | `serving.kserve.io/v1alpha1` | `serving.kserve.io/v1alpha1` | ✅ Identical |
| **K8s API Version** | `apiextensions.k8s.io/v1` | `apiextensions.k8s.io/v1` | ✅ Identical |
| **Controller-gen Version** | `v0.16.2` | `v0.19.0` | ⚠️ Different |
| **API Served** | `true` | `true` | ✅ Identical |
| **Storage Version** | `true` | `true` | ✅ Identical |
| **hostnameOverride Field** | ❌ Not Present | ✅ Present (5 locations) | ⚠️ Different |
| **Base Schema Structure** | Same | Same | ✅ Identical |
| **Status Fields** | Same | Same | ✅ Identical |
| **Printer Columns** | Same | Same | ✅ Identical |

---

## Detailed Differences

### 1. Controller-gen Version

The controller-gen tool version used to generate the CRDs differs between downstream and upstream.

| View | Version | CRD Files Affected |
|------|---------|-------------------|
| **Downstream** | `v0.16.2` | Both LLMInferenceService and LLMInferenceServiceConfig |
| **Upstream** | `v0.19.0` | Both LLMInferenceService and LLMInferenceServiceConfig |

**Impact:** Newer controller-gen versions may include bug fixes, improved validation, or schema enhancements. This version difference accounts for some of the structural improvements in the upstream version.

---

### 2. hostnameOverride Field

The most significant functional difference is the presence of the `hostnameOverride` field in the upstream version.

#### Overview

| Attribute | Details |
|-----------|---------|
| **Field Type** | `string` |
| **Purpose** | Allows overriding the hostname in PodSpec templates |
| **Availability in Downstream** | ❌ Not Present |
| **Availability in Upstream** | ✅ Present in 5 locations |

#### Locations in LLMInferenceService CRD

| # | Location Path | Description |
|---|---------------|-------------|
| 1 | `.spec.prefill.template.hostnameOverride` | Prefill template PodSpec (~line 2087) |
| 2 | `.spec.prefill.template.worker.hostnameOverride` | Prefill worker PodSpec (~line 5855) |
| 3 | `.spec.router.scheduler.pool.spec.template.hostnameOverride` | Scheduler pool template PodSpec (~line 10670) |
| 4 | `.spec.template.hostnameOverride` | Main runtime template PodSpec (~line 14440) |
| 5 | `.spec.worker.hostnameOverride` | Worker PodSpec (~line 18208) |

#### Locations in LLMInferenceServiceConfig CRD

The same 5 locations are present in the LLMInferenceServiceConfig CRD, matching the structure of LLMInferenceService.

**Schema Definition:**
```yaml
hostnameOverride:
  type: string
```

**Use Case:** The `hostnameOverride` field allows users to customize the hostname of pods created from these templates, which can be useful for:
- Network identity and DNS resolution
- Hostname-based routing or filtering
- Debugging and observability
- Compatibility with applications expecting specific hostnames

---

## File Size Comparison

### LLMInferenceService CRD

| View | Line Count | Difference |
|------|-----------|-----------|
| **Downstream** | 19,380 lines | Baseline |
| **Upstream** | 20,085 lines | +705 lines |

### LLMInferenceServiceConfig CRD

| View | Line Count | Difference |
|------|-----------|-----------|
| **Downstream** | 19,270 lines | Baseline |
| **Upstream** | 19,975 lines | +705 lines |

**Explanation:** The upstream version is approximately 3.6% larger due to the addition of the 5 `hostnameOverride` fields and their associated schema definitions.

---

## Schema Fields Comparison

### Identical Fields in spec

The following major spec fields are **identical** between downstream and upstream:

| Field Category | Fields |
|---------------|--------|
| **Base References** | `baseRefs` (with name property) |
| **Model Configuration** | `model.uri`, `model.name`, `model.criticality`, `model.lora.adapters` |
| **Parallelism** | `parallelism.data`, `parallelism.dataLocal`, `parallelism.dataRPCPort`, `parallelism.expert`, `parallelism.pipeline`, `parallelism.tensor` |
| **Deployment** | `replicas` |
| **Router** | `router.gateway`, `router.ingress`, `router.route`, `router.scheduler` |
| **Templates** | All PodSpec fields (containers, volumes, affinity, etc.) except `hostnameOverride` |
| **Worker Configuration** | All worker PodSpec fields except `hostnameOverride` |

### Identical PodSpec Fields

Both versions include the complete PodSpec with **45+ fields**, including:

- `activeDeadlineSeconds`
- `affinity`
- `automountServiceAccountToken`
- `containers`
- `dnsConfig`
- `dnsPolicy`
- `enableServiceLinks`
- `ephemeralContainers`
- `hostAliases`
- `hostIPC`
- `hostNetwork`
- `hostPID`
- `hostUsers`
- `hostname` (standard field, present in both)
- `imagePullSecrets`
- `initContainers`
- `nodeName`
- `nodeSelector`
- `os`
- `overhead`
- `preemptionPolicy`
- `priority`
- `priorityClassName`
- `readinessGates`
- `resourceClaims`
- `resources`
- `restartPolicy`
- `runtimeClassName`
- `schedulerName`
- `schedulingGates`
- `securityContext`
- `serviceAccount`
- `serviceAccountName`
- `setHostnameAsFQDN`
- `shareProcessNamespace`
- `subdomain`
- `terminationGracePeriodSeconds`
- `tolerations`
- `topologySpreadConstraints`
- `volumes`

---

## Status Fields Comparison

### Identical Status Structure

Both downstream and upstream versions have **identical status fields**:

| Field | Type | Description |
|-------|------|-------------|
| `address` | Object | Single address with CACerts, audience, name, url |
| `addresses` | Array | Array of address objects |
| `annotations` | Map | Key-value annotations |
| `conditions` | Array | Standard K8s conditions (lastTransitionTime, message, reason, severity, status, type) |
| `observedGeneration` | Integer | Last observed generation number |
| `url` | String | Primary URL for the service |

---

## Validation Rules Comparison

Both versions contain **identical validation rules**:
- **18 x-kubernetes-validations** rules
- Same validation logic and CEL expressions
- Same enforcement of required fields and constraints

---

## Sample YAML Configurations

### API Version in Samples

All sample YAML files in both downstream and upstream use:

```yaml
apiVersion: serving.kserve.io/v1alpha1
kind: LLMInferenceService
```

### Sample Files Comparison

| Sample Category | Downstream Count | Upstream Count | Status |
|----------------|-----------------|----------------|--------|
| **DP+EP Examples** | 4 files + network config | 4 files + network config | ✅ Same |
| **OPT-125M CPU** | 3 files | 3 files | ✅ Same |
| **Precise KV Cache** | 1 file | 1 file | ✅ Same |
| **Single Node GPU** | 3 files | 3 files | ✅ Same |
| **Setup Guides** | Present (OCP 4.18, OCP GA) | Not Present | ⚠️ Downstream Only |
| **Getting Started** | Present | Not Present | ⚠️ Downstream Only |

**Note:** The downstream-view includes additional setup and getting-started documentation that is not present in the upstream-view.

---

## Conclusions

### Key Findings

1. **API Compatibility:** Both versions support the same API version (`v1alpha1`), ensuring compatibility for users deploying LLMInferenceService resources.

2. **Schema Parity:** The core schema is virtually identical, with 99%+ field parity between downstream and upstream.

3. **Main Difference:** The `hostnameOverride` field is the only functional difference in the CRD schema, present in upstream but absent in downstream.

4. **Tooling Version:** Upstream uses a newer controller-gen version (v0.19.0 vs v0.16.2), which may include improvements in schema generation.

### Recommendations

- **For Downstream Users:** Be aware that `hostnameOverride` is not available. If hostname customization is needed, use the standard `hostname` field or alternative approaches.

- **For Upstream Users:** The `hostnameOverride` field provides additional flexibility for hostname customization across all PodSpec templates.

- **For Migration:** Migrating between downstream and upstream should be straightforward, as long as the `hostnameOverride` field is not used (or removed when moving to downstream).

---

## Additional Resources

- [Downstream README](../../downstream-view/docs/samples/llmisvc/README.md)
- [Upstream Samples](../../upstream-view/docs/samples/llmisvc/)
- [KServe Documentation](https://kserve.github.io/website/)
