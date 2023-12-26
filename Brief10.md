# Brief10

1. **Creation groupe de resources:**

```consol
az group create --location westeurope --resource-group Brief10-Raja 
```

2. **Cree un cluster Aks** 
 
      ```consol
      - az aks create -g Brief10-Raja -n AKSCluster --generate-ssh-key --node-count 1 --enable-managed-identity -a ingress-appgw --appgw-name myApplicationGateway --appgw-subnet-cidr "10.225.0.0/16"   
      ```

3. **Ajouter le credentials**

    ```consol
    az aks get-credentials --name AKSCluster --resource-group Brief10-Raja
    ```

4. **Ajouter Helm repository pour Prometheus et Grafana**

    ```consol
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    ```
5. **Mettre la mis a jours le repository helm**

    ```consol
    helm repo update
    ```
6. **Installer Helm Chert dans le namespace appeler monitoring**

      ```consol
      helm install prometheus \
      prometheus-community/kube-prometheus-stack \
      --namespace monitoring \
      --create-namespace
      ```

7. **Vous pouvez vérifier que la pile a été déployée en exécutant la commande suivante**

      ```consol
      kubectl get all -n monitoring
      ```

8. **Pour exposez Grafana**

    ```consol
    kubectl port-forward svc/prometheus-grafana -n monitoring 4000:80
    ```
  Premier acces: 127.0.0.1:4000 -> utilisateur:admin - mdp:prom-operator.

  

9. **Pour exposez Prometheus**
  
    ```consol
    kubectl port-forward svc/prometheus-kube-prometheus-prometheus -n monitoring 4001:9090
    ```

 7. **Alert Redis**

      ```consol
      helm upgrade redis -f values.yaml prometheus-community/prometheus --install 
      ```

    
