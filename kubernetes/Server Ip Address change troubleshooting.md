
# Kubernetes IP Change Recovery Guide

## 1. Update kube-apiserver Configuration
First, check and update the API server configuration:
```
sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml
```
## 2. Update kubelet Configuration
Edit the kubelet configuration:

```
sudo vi /etc/kubernetes/kubelet.conf
```

## 3. Regenerate Certificates (if needed)
If your certificates were bound to the old IP, you may need to regenerate them:

```
sudo kubeadm init phase certs all --apiserver-advertise-address <NEW_IP>
```

## 4. Update kubeconfig Files
Update all kubeconfig files that reference the old IP:

```
sudo find /etc/kubernetes/ -name "*.conf" -exec sed -i "s/<OLD_IP>/<NEW_IP>/g" {} \;
```

## 5. Update kubeconfig Files
Update your user kubeconfig:

```
sed -i 's/192.168.149.128/<NEW_IP>/g' ~/.kube/config
```

## 6. Regenerate Certificates (Critical Step)
The certificates likely contain the old IP in their SAN (Subject Alternative Names). You'll need to:

```
sudo kubeadm certs renew all --config=/etc/kubernetes/kubeadm-config.yaml
```
If you don't have the kubeadm config file, you might need to recreate it or use:

```
sudo kubeadm init phase certs all --apiserver-advertise-address <NEW_IP>
```

## 7. Certificate Renewal
```
sudo cp -r /etc/kubernetes/pki /etc/kubernetes/pki.backup
```

## 8. Regenerate all certificates with new IP
```
sudo kubeadm certs renew all --config=/etc/kubernetes/kubeadm-config.yaml
```
## 9. Update All Components
```
sudo kubeadm init phase control-plane all --apiserver-advertise-address=<NEW_IP>
sudo kubeadm init phase kubeconfig all --apiserver-advertise-address=<NEW_IP>
```

## 10. Manually verify certificate SANs
```
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout | grep -A1 "Subject Alternative Name"
Ensure your new IP appears in the output.
```
## 11. Regenerate kubeconfig files
```
sudo kubeadm init phase kubeconfig all \
  --apiserver-advertise-address=<NEW_IP> \
  --cert-dir /etc/kubernetes/pki
```
## 12. Update client configurations
```
cp /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
```

## 13. Restart Components
Restart the affected services:
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

## 14. Verify API Server Accessibility
Check if the API server is now reachable at the new address:

```
curl -k https://<NEW_IP>:6443/api
```

