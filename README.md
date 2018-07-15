# weechat-helm #

<http://github.com/lrvick/weechat-helm>

## About ##

This helm chart spins up a weechat container with a running relay and
persistent disk.

## Requirements ##

  * minikube (for dev/testing)
  * kubectl
  * helm

## Testing ##

1. Start Minikube Ingress and Helm

    ```
    minikube start
    minikube addons enable ingress
    helm init
    ```

2. Install Helm dependencies and chart

    ```
    helm install -n weechat .
    ```

3. Monitor progress of weechat initialization

    ```
    kubectl -n weechat logs \
      -f $(kubectl -n weechat get pods -l app=weechat -o name)
    ```

4. Add local DNS entry for minikube

    ```
    echo "$(minikube ip) weechat.local" | sudo tee -a /etc/hosts
    ```

5. Connect with ssh

    ```
    ssh weechat.local
    ```

## Production Deployment ##

1. Install helm chart

    ```
    helm install -n weechat .
    ```

2. Ensure weechat is pinholed to outside world via ingress such as nginx

    ```
    helm install \
      --tls \
      --name nginx-ingress \
      --namespace kube-system \
      stable/nginx-ingress \
      --set rbac.create=true \
      --set tcp.22="default/weechat:22"
      --set tcp.9000="default/weechat:9000"
    ```
