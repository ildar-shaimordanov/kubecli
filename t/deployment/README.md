# Classical way

Create a new namespace

```
kubectl create ns foo-bar
```

Create a new deployment in the specified namespace

```
kubectl -n foo-bar apply -f deployment-hello.yaml
```

List the particular pods

```
kubectl -n foo-bar get pods
```

Return logs from all pods in the deployment

```
kubectl -n foo-bar logs -l app.kubernetes.io/name=hello
```

# New approach

Create a new namespace

```
kubecli create ns foo-bar
```

Set the namespace

```
KUBECLI_NS="foo-bar"
```

Create a new deployment in the specified namespace

```
kubecli apply -f deployment-hello.yaml
```

List the particular pods

```
kubecli get pods
```

Return logs from all pods in the deployment

```
kubecli logs -l app.kubernetes.io/name=hello
```

or

Set the selector once

```
KUBECLI_SEL="app.kubernetes.io/name=hello"
```

run the following command

```
kubecli tail
```
