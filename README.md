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
    helm dependency update
    helm install -n weechat .
    ```

3. Monitor progress of weechat initialization

    ```
    kubectl -n weechat logs \
      -f $(kubectl -n weechat get pods -l app=weechat -o name)
    ```

4. Add local DNS entry for minikube

    ```
    sudo echo "192.168.99.100 weechat.local" >> /etc/hosts
    ```

5. Connect with ssh

    ```
    ssh weechat.local
    ```

## Production Deployment ##

TODO
