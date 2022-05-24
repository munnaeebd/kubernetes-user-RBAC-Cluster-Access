# kubernetes-user-RBAC-Cluster-Access

## kubernetes RBAC, User management 

https://www.openlogic.com/blog/granting-user-access-your-kubernetes-cluster#:~:text=Kubernetes%20doesn't%20manage%20users,level%20security%20(TLS)%20certificates.
https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#normal-user

## generate csr with CN munna
```
openssl req -new -newkey rsa:4096 -nodes -keyout munna-k8s.key -out munna-k8s.csr -subj "/CN=munna/O=devops"


cat bob-k8s.csr | base64 | tr -d '\n'

## copy the key to the below file

cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: munna-user
spec:
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJRVpqQ0NBazRDQVFBd0lURU9NQXdHQTFVRUF3d0ZiWFZ1Ym1FeER6QU5CZ05WQkFvTUJtUmxkbTl3Y3pDQwpBaUl3RFFZSktvWklodmNOQVFFQkJRQURnZ0lQQURDQ0Fnb0NnZ0lCQU0weWFTdUJvVDNoUHpHSTFQL0krK3cxCkhOMnhPczBXdW9FNENCWDBJQ1FJVGNaakk2M3RMMXBrM0xub1B1WmFTMm9aTmh0emorS2JrY2tHSXRxTmFJelYKMjgyLzB0YnYxRkhnc2ZrVUR5YXRoSjBLOVAzZXlaZWUvL04ralF4ZXl1SVFoS1V4U1ZhdkdFQlJZQitZbTFmbwpNRHpMVTV0ZDBwZGd2TWwrQVFRanVhVVMxVkFpQjlGRVB6MXhNZXJYS1BsNXRFcmdUMUkwZkFVaDM0dTRwS2o3CmJxWXFMU015aUlvc0o2dkEzNHRBMGR6eWxyVkVLVDFqWC9JMEVMZTRVb0crTUlaTEhmL2Zkd3haTitJd2h4RlEKTmk2N0VRK0dDRE1YVlk1ZjA1NUUxTmhWZ0NFVUZaQmdDTDgrZkJqcmxGSGdsUkFKbUkrdm9pWGdGeHMzb0VDZgpBdC9iUFY5eWJoazNOT1krTTNYRCtwcjZaMVdiQmQ1UUR0WHFTc3BEbFA1VFRBZlRQT0xjYTc5c1ZRVDRaQ3puCkdvdDdlcHd5dVVFUTY5VzYyWkI1bHlHS2FuVzk1UWVvdkNmT0Y0L1MzczA2QnJnRy8vUWZmS0lmUTRYSGQ3bk8KV2E1emc4YWx2OFNWOE82Q3FZTE1YdS81c2Z6YzlBUVA5UkhRSTk3Szh6L016R0t2NHpzZkRNMHFuS05HajNvVgo0Z0d0MjllK1VvRW5DRjQvZ2lkUHBkSUF5bnN5V1ZyeHNyUmdxV2d6aFdlRFVONzdzRWZ6bCtadEZUQ1VHTGpWCkFtcUViS0hSRC85SDBrcUZwZUZPT0RuVmVtMk10UTV0ekU1Z1JUU1gyaUVWakEvV2JCT0ZObHdDWG5GdnBiUHgKTGJKbVAxQ3lubVg3Y0ZtbWhxVlBBZ01CQUFHZ0FEQU5CZ2txaGtpRzl3MEJBUXNGQUFPQ0FnRUFObjM4RzQrNgpHZnJCdS94eWE2UTdUNXB1UWpjVVliWXZpTUs5ZWs1bzBqZ0tFU1BKN0h0djlFL1k3d2V1Z1QvUjUxc3FVR0x6CjRVOHB4TVNFZmkwWGtST1dLTlZETVFzNk9NTnRSNWF4aEVXN2kyWTlIU1JUelVxQlJHQ21ISUdHVFRHYmZSYVgKejNndVdQdFNiSDZGU2FyeXhvSUIxU3VHSkNGczk4dTkxNlA5M2k4MDlNU0tBRmkzZDRnSTdUSnFXUzRrcURubApPU281ZEkrRTRPdllZb1B4MW5uYldlYzhValNnSDJCeXN5dE16cCtBZUhWRnUxZnRZTmxiZ2N0U2p0eWczZ29wCmN0MjJ2Z0cvb0diZlhxNVJ3elNJYWpGVUk5bVBLdmRCd29IWERqVkFpeWtGdjVwemYxL2tLR3VCT05valIyTHMKM3BhOEpOcGVHbVl2UUphYU83a1RlTHlESkZVMnFGNjVzYkl1d2RXbXRIWVpCblNDMFVCb3NVYVZWcktjSTJKQQpnLzlGbHBkRGZVRUZ2WUtiSUFWMHgyQ1ZxTk1kRlV4Vm1BcDhXNGN4OGlzU0NTTDRHZDdOT3h2SExBK1d1SzJaCklYRXE1d2s2OGtkTVQ2SThJU0FFUGh4UmNEQVA2dVVXalMzWXdBY3l6a2NUUHdpN1BMVkhkQUFseWI0ak52NEkKNjRFbVdEZitUTFZJem4yUlVkcVQ4VTFLZXN5WG1oMGZ0QlBmQkhneENtTFJpWmdRL3E0K2ZDOUxta0NIeWpXZApLZHd2UGh0UDVHQ2I0THdGcENyKzV5YlRaVjZRMjR5TTdxbklUb0JrYTNoNUxDblRmTk9zaDhVc3FnaVpoODIxCngybExRUUoyS3BhQVdVVk55UGpHdHJRK2dOajJyb2s5b0lFPQotLS0tLUVORCBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0K
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF
````

## Approve the Certificate, Generate kubeconfig file, role rolebinding, clusterrolebinding and assign to user-munna

```
kubectl certificate approve munna-user

kubectl get csr munna-user -o jsonpath='{.status.certificate}' | base64 --decode > munna-k8s.crt

kubectl config view -o jsonpath='{.clusters[0].cluster.certificate-authority-data}' --raw | base64 --decode - > k8s-ca.crt

kubectl config set-cluster $(kubectl config view -o jsonpath='{.clusters[0].name}') --server=$(kubectl config view -o jsonpath='{.clusters[0].cluster.server}') --certificate-authority=k8s-ca.crt --kubeconfig=munna-k8s-config --embed-certs

kubectl config set-credentials munna --client-certificate=munna-k8s.crt --client-key=munna-k8s.key --embed-certs --kubeconfig=munna-k8s-config
kubectl config set-context munna --cluster=$(kubectl config view -o jsonpath='{.clusters[0].name}') --namespace=munna --user=munna --kubeconfig=munna-k8s-config

## Role Set: 

kubectl create ns munna
kubectl create rolebinding munna-admin --namespace=munna --clusterrole=admin --user=munna

Impact: using munna user and kubeconfig we will get only munna namespace access

or 
kubectl create role devops --verb=create --verb=get --verb=list --verb=update --verb=delete --resource=pods
kubectl create rolebinding devops-binding-munna --role=devops --user=munna

Impact: using munna user and kubeconfig we will get only default namespace access as role and rolebinding is created in default namespaces

or 

Full access cluster 

kubectl create clusterrolebinding root-cluster-admin-binding --clusterrole=cluster-admin --user=munna
```





