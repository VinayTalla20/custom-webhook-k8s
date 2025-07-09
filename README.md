# 🔐 custom-webhook-k8s

A custom Kubernetes admission controller built in Python for **validating** or **mutating** configurations using webhooks.

---

## 📦 Docker Image

### Build & Push Docker Image

```bash
docker build -t vinaytalla/webhook-validate:v2 .
docker push vinaytalla/webhook-validate:v2
🚀 Deploy Webhook to Kubernetes
1. Deploy Admission Controller
bash
Copy
Edit
kubectl apply -f admission-controller-deployment.yaml
2. Create Kubernetes Service (port 443)
bash
Copy
Edit
kubectl apply -f admission-controller-service.yaml
🔐 TLS Configuration
3. Generate Self-Signed Certificates
Use openssl to generate a certificate for the webhook.

bash
Copy
Edit
openssl req -x509 -sha256 -newkey rsa:2048 \
  -keyout webhook.key -out webhook.crt -days 365 -nodes \
  -addext "subjectAltName = DNS.1:validate.webhook.svc"
4. Create Secret for TLS Certificates
bash
Copy
Edit
kubectl create secret tls webhook-certs \
  --cert=webhook.crt --key=webhook.key \
  -n validate
📌 Replace validate with your appropriate namespace.

⚙️ Configure the Webhook
5. Apply ValidatingWebhookConfiguration
bash
Copy
Edit
kubectl apply -f webhook-validating.yaml
Ensure this configuration points to:

The correct service DNS (validate.webhook.svc)

Port 443

TLS configuration

✅ Test the Webhook
6. Apply a Sample Test Deployment
bash
Copy
Edit
kubectl apply -f test-validate-webhook.yaml
If everything is working correctly:

The webhook will intercept and validate the deployment request.

It may allow or reject it based on your logic in the Python controller.

📁 File Structure
css
Copy
Edit
.
├── Dockerfile
├── main.py
├── admission-controller-deployment.yaml
├── admission-controller-service.yaml
├── webhook-validating.yaml
├── admission-controller-secret.yaml
├── test-validate-webhook.yaml
├── webhook.crt
├── webhook.key
📚 References
Kubernetes Webhooks

OpenSSL SAN Certificates

🧠 Notes
You can extend this to mutating webhooks by modifying the logic and replacing the ValidatingWebhookConfiguration with MutatingWebhookConfiguration.

Ensure your cluster supports admissionregistration.k8s.io/v1.

👤 Author
Vinay Talla

Contributions & PRs welcome!
