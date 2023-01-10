# kubernetes GitOps

## GitOps란

### GitOps란

- Git + Operation
- 서비스, 인프라에 대한 형상을 Git Repo로 관리
- Git Log와 Diff를 통한 이력관리
- Pull Request 통해 작업 리뷰, 승인 받은 후에만 배포
### GitOps의 장점
- 장점
  - 개발자 친화적
    - Git을 사용하므로 개발자가 익숙
  - 코드 리뷰
    - 배포전 리뷰 가능
  - 타겟과 소스의 분리
    - 타겟에 접근할 필요 없이 Git Repo를 통해 타겟 변경
  - 타겟의 내용은 Git과 항상 같다
    - 형상에 대해 의심하지 않아도 된다 
### GitOps의 원칙
- Declaratively
    - 쿠버네티스의 모든 리소스는 선언적으로 관리됩니다.
    - 선언적 관리: 리소스에 대한 정의와 실제 리소스가 동일
    - Git에 저장된 이러한 선언적 리소스를 쿠버네티스에 적용하면 배포는 알아서 진행
- Originality
    - 배포되는 리소스의 유일한 저장소로 Git을 사용
    - Git의 저장된 리소스와 배포된 리소스를 일치
- Diff Detection
    - GitOps 컨트롤러는 Git에 저장된 리소스와 인프라에 배포된 리소스를 모디터링
    - 모니터링 대상: 리소스간의 차이점
- Auto Sync
    - GitOps 컨트롤러는 Git에 저장된 리소스와 인프라에 배포된 리소스가 다를 경우 이를 자동으로 동기화