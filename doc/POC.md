# Покрокове розгортання ArgoCD на k3d Kubernetes кластері

### Крок 1: Встановлення k3d

1. Встановлюємо k3d на вашу локальну машину, якщо його ще немає:
```
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```
Перевірка версії k3d
```
k3d version
```
2. Створюєм новий кластер k3d:
```
k3d cluster create <cluster_name>
```
Перевірка, чи працює кластер k3d:
```
kubectl get nodes
```

### Крок 2: Встановлення ArgoCD на Kubernetes
1. Додаєм ArgoCD namespace:
```
kubectl create namespace argocd
```
2. Встановлюємо  ArgoCD за допомогою kubectl:
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```
3. Перевіряємо, що всі компоненти ArgoCD запущені:
```
kubectl get pods -n argocd
```

### Крок 3: Налаштування доступу до ArgoCD UI
1. Запускаємо порт для доступу до інтерфейсу:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
2. Отримуємо початковий пароль для входу:
```
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```
Логін: ```admin```

3. Веб-інтерфейс ArgoCD: http://localhost:8080. Використовуємо логін ```admin``` та пароль, який отримали на 3.2 кроці.