# tunnelchart

## install

git clone https://github.com/openoms/tunnelchart

helm template tunnelchart --debug

helm show all tunnelchart --debug

helm install tunnel tunnelchart

## get to the shell in the pod

kubectl exec -it tunnel -- sh


## Helper script to run on the host:

```
k8stunnel() {
    POD="$1"
    HOSTPORT="$2"
    PODPORT="$3"
    if [ -z "$POD" -o -z "$HOSTPORT" -o -z "$PODPORT" ]; then
    	echo "Usage: k8stunnel [pod name] [container] [host port] [pod port]"
        return 1
    fi
    pkill -f 'nc 127.0.0.1 "$HOSTPORT"'
    kubectl exec -it "$POD" -- apk add ucspi-tcp6
    nc 127.0.0.1 "$HOSTPORT" | microk8s kubectl exec -it "$POD" -- tcpserver 127.0.0.1 "$PODPORT" cat &
    echo "To access the host:"$HOSTPORT" Connect to 127.0.0.1:"$HOSTPORT" in the pod"
}
```

## Forward the bitcoind testnet ports from the host to the tunnel pod:

```
k8stunnel tunnel 18332 18332
k8stunnel tunnel 21332 21332
k8stunnel tunnel 21333 21333
```
