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
