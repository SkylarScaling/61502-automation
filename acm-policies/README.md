# ACM Policies

Each directory defines Policies and Policy Sets that can be applied to target clusters by ACM using ACM Application Subscriptions.

## PolicyGenerator
`policyGenerator.yaml`, which is referenced from the accompanying `kustomization.yaml`, defines the structure of the ACM policies and policy sets that comprise the ACM Application that will be created by a Subscription to this repo.

A subscription to an Application in this repo might look like this:

    apiVersion: v1
    kind: Namespace
    metadata:
      name: acm-install-policies
    ---
    apiVersion: app.k8s.io/v1beta1
    kind: Application
    metadata:
      name: acm-install-policies
      namespace: acm-install-policies
    spec:
      componentKinds:
        - group: apps.open-cluster-management.io
          kind: Subscription
      selector:
        matchExpressions:
          - key: app
            operator: In
            values:
              - acm-install-policies
    ---
    apiVersion: apps.open-cluster-management.io/v1
    kind: Channel
    metadata:
      annotations:
        apps.open-cluster-management.io/reconcile-rate: medium
      name: acm-install-policies-channel
      namespace: acm-install-policies
    spec:
      type: Git
      pathname: https://github.com/skylarscaling/61502-automation.git
    ---
    apiVersion: apps.open-cluster-management.io/v1
    kind: Subscription
    metadata:
      annotations:
        apps.open-cluster-management.io/git-branch: main
        apps.open-cluster-management.io/git-path: acm-policies/install-acs
        apps.open-cluster-management.io/reconcile-option: replace
      labels:
        app: acm-install-policies
      name: acm-install-policies-subscription
      namespace: acm-install-policies
    spec:
      channel: acm-install-policies/acm-install-policies-channel
      placement:
        placementRef:
          kind: PlacementRule
          name: acm-install-policies-placement
    ---
    apiVersion: apps.open-cluster-management.io/v1
    kind: PlacementRule
    metadata:
      labels:
        app: acm-install-policies
      name: acm-install-policies-placement
      namespace: acm-install-policies
    spec:
      clusterSelector:
        matchLabels:
          name: local-cluster
Notes:
- The User or ServiceAccount that creates the Subscription for an ACM Applicaiton needs to hava the open-cluster-management:subscription-admin ClusterRoleBinding:


    kind: ClusterRoleBinding  
    apiVersion: rbac.authorization.k8s.io/v1  
    metadata:  
      name: 'open-cluster-management:subscription-admin'
    subjects:
      - kind: User
        apiGroup: rbac.authorization.k8s.io
        name: admin
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: 'open-cluster-management:subscription-admin'

- The User or ServiceAccount should also be added to system:image-puller for the project:


    kind: ClusterRoleBinding  
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      name: sky-image-pull-binding
    subjects:
      - kind: User
        apiGroup: rbac.authorization.k8s.io
        name: admin
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: 'system:image-puller'