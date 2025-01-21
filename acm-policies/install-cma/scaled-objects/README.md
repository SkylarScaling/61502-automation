# scaled-objects

This directory contains manually created, or automatically generated, ScaledObjects for use by the Custom Metrics Autoscaler.

## ScaledObject
An example ScaledObject might look like the following:

    apiVersion: keda.sh/v1alpha1
    kind: ScaledObject
    metadata:
        name: namespace-name-scaled-object
        namespace: deployment-namespace
    spec:
        cooldownPeriod: 0
        maxReplicaCount: 5
        minReplicaCount: 0
        scaleTargetRef:
            name: deployment-name
        triggers:
            - metadata:
                desiredReplicas: 2
                start: 0 6 * * 1,2,3,4,5
                end: 0 18 * * 1,2,3,4,5
                timezone: US/Eastern
            type: cron

## References
[Custom Metrics Autoscaler (CMA) - OpenShift Docs](https://docs.openshift.com/container-platform/4.17/nodes/cma/nodes-cma-autoscaling-custom.html)

[Cron ScaledObject - KEDA Docs (CMA Upstream)](https://keda.sh/docs/2.16/scalers/cron/)