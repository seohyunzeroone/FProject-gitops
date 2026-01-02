# FProject GitOps Repository

이 저장소는 FProject 애플리케이션의 Kubernetes 배포 매니페스트를 관리합니다.

## 📁 디렉토리 구조

```
.
├── environments/
│   ├── dev/          # 개발 환경 매니페스트
│   ├── staging/      # 스테이징 환경 매니페스트
│   └── prod/         # 프로덕션 환경 매니페스트
└── argocd/           # ArgoCD Application 정의
```

## 🚀 사용 방법

### 1. 환경별 매니페스트 수정
- `environments/dev/` - 개발 환경 설정
- `environments/staging/` - 스테이징 환경 설정
- `environments/prod/` - 프로덕션 환경 설정

### 2. Git에 커밋 및 푸시
```bash
git add .
git commit -m "Update deployment configuration"
git push
```

### 3. ArgoCD가 자동으로 변경사항 감지 및 배포
- ArgoCD는 이 저장소를 모니터링합니다
- 변경사항이 감지되면 자동으로 EKS 클러스터에 배포합니다

## ⚠️ 주의사항

### 보안
- **Secret 파일에는 실제 비밀번호를 커밋하지 마세요**
- 민감한 정보는 GitHub Secrets 또는 AWS Secrets Manager 사용
- base64로 인코딩된 값도 평문과 동일하게 취급됩니다

### 이미지 태그
- 이미지 태그는 CI 파이프라인에서 자동으로 업데이트됩니다
- 수동으로 변경하지 마세요

### 환경별 설정
- 각 환경은 독립적으로 관리됩니다
- 환경별로 다른 리소스 제한, 레플리카 수 등을 설정할 수 있습니다

## 📝 매니페스트 파일 설명

### Deployment
- 애플리케이션 파드 정의
- 레플리카 수, 이미지, 환경 변수 설정

### Service
- 네트워크 노출 설정
- LoadBalancer 또는 ClusterIP 타입

### ConfigMap
- 비민감 설정값 저장
- 데이터베이스 호스트, 포트 등

### Secret
- 민감한 정보 저장 (base64 인코딩)
- 실제 값은 CI 파이프라인에서 주입

## 🔄 배포 프로세스

1. 개발자가 코드를 푸시
2. GitHub Actions가 Docker 이미지 빌드 및 ECR 푸시
3. GitHub Actions가 이 저장소의 이미지 태그 업데이트
4. ArgoCD가 변경사항 감지
5. ArgoCD가 EKS 클러스터에 자동 배포

## 🛠️ 트러블슈팅

### ArgoCD 동기화 실패
```bash
# ArgoCD 애플리케이션 상태 확인
argocd app get fproject-backend

# 수동 동기화
argocd app sync fproject-backend
```

### 매니페스트 검증
```bash
# YAML 문법 검증
kubectl apply --dry-run=client -f environments/dev/
```

## 📚 참고 자료

- [Kubernetes 문서](https://kubernetes.io/docs/)
- [ArgoCD 문서](https://argo-cd.readthedocs.io/)
- [GitOps 원칙](https://www.gitops.tech/)
