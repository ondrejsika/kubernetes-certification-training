[Ondrej Sika (sika.io)](https://sika.io) | <ondrej@sika.io>

# kubernetes-certification-training

## Vim

Open file

```
vim <file>
```

Write and quit

```
:wq
```

Quit without saving

```
:q!
```

Start editing

```
i
```

Exit editing

```
Esc
```

Open folfer

```
vim <folder>
```

then open file with selecting it and pressing `o`

Search

```
/<keyword>
```

Delete line

```
dd
```

Go to end of the file

```
G
```

Delete from current line to end of the file

```
dG
```

Go to beginning of the file

```
gg
```

Delete from current line to beginning of the file

```
dgg
```

Go to line number

```
:<line-number>
```

## CLI Tools

### grep

Filter output

```
kubectl get po -A | grep <keyword>
```

- `grep -A5 <keyword>` to show 5 lines after match (`A` stands for after)
- `grep -B5 <keyword>` to show 5 lines before match (`B` stands for before)
- `grep -C5 <keyword>` to show 5 lines before and after match (`C` stands for context)

### less

- Search output with `/<keyword>`
- Navigate with `n` for next and `N` for previous match

### head

Show first lines of output

```
kubectl create --help | head -n 40
```

### tail

Show last lines of output

```
kubectl logs -n <namespace> <pod-name> | tail -n 10
```

### base64

Encode plain text to base64:

```
echo -n 'hello' | base64
```

Decode base64 to plain text:

```
echo -n 'aGVsbG8=' | base64 -d
```

### openssl

Check Validity of TLS Certificate

```
openssl x509 -in <file.crt> -noout -text | grep -A2 'Validity'
```

for example:

```
openssl x509 -in /var/run/secrets/kubernetes.io/serviceaccount/ca.crt -noout -text | grep -A2 'Validity'
```

## Logs

Try to get logs from pods:

```
kubectl get pod -A
```

```
kubectl logs -n <namespace> <pod-name>
```

Try to get logs from containers:

```
crictl ps -a
```

```
crictl logs <container-id>
```

Try to get sytem logs:

```
journalctl -xe
```

You can also check log files directly:

- `/var/log/pods`
- `/var/log/containers`
- `/var/log/syslog`


## Filter logs

Using grep

```
journalctl -xe | grep <keyword>
```

for example:

```
journalctl -xe | grep api-server
```

or search with less

```
journalctl -xe | less
```

and then search with `/` key, for example `/api-server`. then navigate with `n` for next and `N` for previous match.

## Debug Pods

1. check pod statuses

```
kubectl get po -A
```

2. check events in pod description

```
kubectl describe po -n <namespace> <pod-name>
```

3. check logs

```
kubectl logs -n <namespace> <pod-name>
```

## Common Pod Issues

### CrashLoopBackOff

Pod is crashing and restarting in a loop. Check logs to find out why.

### ImagePullBackOff / ErrImagePull

Pod cannot pull the image. Check if the image name is correct and if the image is available in the registry.

### ContainerConfigurationError

Check the description of the pod to find out more.

### Pending

Pod is not scheduled.

- Check if there are any node name, node selectors or affinity rules that prevent scheduling.
- Check if there are any taints on the nodes that prevent scheduling.
- Check if there are enough resources in the cluster.

## Save Time during the Exam

### Kubectl Help

```
kubectl <command> -h
```

with search

```
kubectl <command> -h | grep -A5 <keyword>
```

```
kubectl <command> --help | less
```

for example:

```
kubectl expose -h | grep -A5 'port'
```

### Kubectl Get Output

Basic output

```
kubectl get po
```

Wider output, more columns

```
kubectl get po -o wide
```

Only resource names

```
kubectl get po -o name
```

YAML

```
kubectl get po <pod-name> -o yaml
```

YAML to file

```
kubectl get po <pod-name> -o yaml > pod.yaml
```

Custom columns

```
kubectl get po -o custom-columns='NAME:.metadata.name,IMAGE:.spec.containers[0].image'
```

### Dry Run and Output to YAML

You can use dry run to generate YAML files for various resources. Very useful during the exam.

```
kubectl <command> <resource> --dry-run=client -o yaml > file.yaml
```

### Kubectl Create

List available resources to create

```
kubectl create --help | less
```

or with head

```
kubectl create --help | head -n 40
```

### Kubectl Create Dry Run

For creating a deployment

```
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > d.yaml
```

If you want to create a DaemonSet or StatefulSet, create a deployment first and then change the kind in the generated YAML file.

#### Create Ingress

```
kubectl create ingress <name> --class=nginx --rule='<host>/=<service>:<port>' --dry-run=client -o yaml > ing.yaml
```

for example:

```
kubectl create ingress my-ingress --class=nginx --rule='example.com/=nginx:80' --dry-run=client -o yaml
```

#### Create ConfigMap

```
kubectl create configmap my-cm --from-literal=key=value --dry-run=client -o yaml > cm.yaml
```

### Kubectl Run Dry Run

For creating a pod

```
kubectl run nginx --image=nginx --dry-run=client -o yaml > p.yaml
```

### Kubectl Expose Dry Run

For creating a service. The deployment must exist first.

```
kubectl expose deployment nginx --port=80 --dry-run=client -o yaml
```
