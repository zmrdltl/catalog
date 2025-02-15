# GitLab Template Guide

1. Template 생성
```bash
kubectl apply -f template.yaml
```

2. TemplateInstance 생성
```bash
kubectl apply -f instance.yaml
```

## Parameter 설명
- APP_NAME  
: GitLab Deployment 제목

- STORAGE  
: GitLab용 PersistentVolumeClaim 크기

- STORAGE_CLASS  
: GitLab용 PVC의 Storage Class (default: csi-cephfs-sc)

- SERVICE_TYPE  
: GitLab용 서비스 종류 (ClusterIP/NodePort/LoadBalancer/Ingress)

- EXTERNAL_URL  
: SERVICE_TYPE으로 정의된 기본 엔드포인트와 다른 엔드포인트를 사용하고 싶을 때 해당 엔드포인트 명시 (http:// 포함)  
(e.g., LoadBalancer로 생성 후 및 LoadBalancer IP에 도메인 등록 후 도메인으로 접속하고 싶은 경우)

- WEB_NODE_IP
: SERVICE_TYPE에 NodePort를 사용하는 경우, 깃랩 접속에 사용할 IP 명시.
: 명시되지 않을 경우 Pod가 배치된 노드의 IP 사용 (접속 자체는 모든 Node의 IP를 통해 가능함)

- WEB_NODE_PORT  
: SERVICE_TYPE에 NodePort를 사용하는 경우, 사용할 NodePort 명시.
: 명시되지 않을 경우 자동으로 배정되는 NodePort 사용 (Service 객체를 통해 포트 확인 필요)

- INGRESS_HOST  
: SERVICE_TYPE에 Ingress를 사용하는 경우, Host 명시.
: 명시되지 않을 경우 `<APP_NAME>.<네임스페이스>.<Ingress 컨트롤러 IP>.nip.io` 사용

- SSH_PORT  
: SSH 포트 포워딩
: 명시되지 않을 경우 2221 포트 사용

- RESOURCE_CPU / RESOURCE_MEM  
: 컨테이너 리소스 request/limit

- KEYCLOAK_URL  
: 키클록 URL (`http://`또는 `https://` 포함)

- KEYCLOAK_CLIENT  
: 키클록 클라이언트 이름

- KEYCLOAK_SECRET  
: 키클록 클라이언트 Credential

- KEYCLOAK_TLS_SECRET_NAME  
: 키클록 TLS 인증서 시크릿 이름

- TLS_SECRET_NAME
: 사용자 TLS 인증서 시크릿 이름

## 키클록 연동 방법
1. 키클록에서 클라이언트 생성
- Name  
: `gitlab`
- Client-Protocol  
: `openid-connect`
- AccessType  
: `confidential`
- Valid Redirect URIs  
: `*`

2. 시크릿 복사
- `Client > gitlab > Credentials > Secret` 복사
