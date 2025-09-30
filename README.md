[Ondrej Sika (sika.io)](https://sika.io) | <ondrej@sika.io>

# kubernetes-certification-training

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
