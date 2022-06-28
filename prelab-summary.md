# Cloud Platfrom Eng. 영역 Pre-Lab PaaS_AWS 과정

- **개요**
  - 본 예제는 GitOps기반 SRE과정을 위한 클라우드 플랫폼 환경구축과 마이크로서비스 전 라이프사이클(분석/설계, 구현, DevOps 툴체인(파이프라인)을 통한 배포, 운영/모니터링)을 커버하도록 구성된 예제입니다. 
  - 분석/설계/구현을 포함한 클라우드 플랫폼기반 운영(컨테이너 오케스트레이션, 모니터링, 로깅, etc) 중심으로 예제 프로젝트는 전개됩니다.
  - PreLab 기간동안 주어진 마이크로서비스 리소스를 사용하거나, 새로운 도메인 모델을 선정 후 설계/구현/배포/운영/모니터링을 적용해도 됩니다.
 
- **진행내역** 
  - MSA 아키텍처 구성
    - ![image](https://user-images.githubusercontent.com/86272090/175823963-3f5fdf90-4192-47e0-8f70-00950435e303.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175823006-8fb6699e-7e8b-4553-9ad4-22cf50539720.png)
    - CI/CD 파이프라인
    - ![image](https://user-images.githubusercontent.com/86272090/176066426-c8a70548-15a2-4c4e-a321-b3b2b36ebbe2.png)



  - Cloud Platform 프로비저닝
    - "EKS 생성"
    - Bastion Host 에서 IAM 보안  자격증명 설정 진행 및
    - eksctl 를 활용하여 t3.medium 용량의 Worker Node 3기 AWS 클러스터 생성 진행
    - ![image](https://user-images.githubusercontent.com/86272090/174931479-31ad8210-346e-46ee-ae97-6f4055a5680e.png)
    - AWS 클러스터 토큰 가져오기
    - ![image](https://user-images.githubusercontent.com/86272090/174934173-32ddb4d1-27be-4ca0-a84b-e52b7f1fce60.png)
    - "ECR 생성" (터미털에서 생성 테스트)
    - ![image](https://user-images.githubusercontent.com/86272090/174934535-462fb32b-a6c8-4a36-a87e-9917d4d6eff3.png)
    - Docker Login to ECR 확인
    - ![image](https://user-images.githubusercontent.com/86272090/174935084-f3d0b4a3-62e0-49f1-bcac-0b0b9211ac43.png)
    - "Metric Server 설치"
    - ![image](https://user-images.githubusercontent.com/86272090/174935276-5f89fd5d-8384-4312-b4c9-512fd786546e.png)



  - DevOps Toolchain 구축 
    - "프로젝트 별 ECR 생성"
    - ![image](https://user-images.githubusercontent.com/86272090/174936592-b5d44c63-03a8-4a81-91da-7aeec5396afe.png)
    - "프로젝트 별 CodeBuild 생성"
    - ![image](https://user-images.githubusercontent.com/86272090/174979131-6be71a3d-93ba-461b-9760-2ef7703f0420.png)
    - CodeBuild 상세
    - Webhook 설정을 통하여 소스 변경 Trigger 로 자동 Build/배포 수행
    - ![image](https://user-images.githubusercontent.com/86272090/174979460-9d766f1c-f9dc-43c5-a59a-d0bcfc64d1fd.png)
    - ![image](https://user-images.githubusercontent.com/86272090/174979584-1538436e-3f73-458e-8589-d35d9a894bfc.png)
    - Maven 으로 Build 시 라이브러리를 캐시화하여 Build 속도 향상 
    - ![image](https://user-images.githubusercontent.com/86272090/174979731-069ef006-5b8a-484c-b7c4-b719e11b8114.png)
    - buildspec-kubectl.yml 상세
    - 1. install : install kubectl
    - 2. pre_build : Logging in to Amazon ECR
    - 3. build : Building the Docker image
    - 4. post_build : Pushing the Docker image, connect kubectl, kubectl apply(배포)
    - ![image](https://user-images.githubusercontent.com/86272090/174980351-c5ab4f39-7661-41fb-94a9-357e1aa87c20.png)
    - ![image](https://user-images.githubusercontent.com/86272090/174980633-cb43ed83-e887-4556-a834-b48ef0371693.png)
    - ![image](https://user-images.githubusercontent.com/86272090/174980728-ee1f5a98-5b03-491a-aec8-cf6e54c6294b.png)
    - EKS Connection 설정
    - ![image](https://user-images.githubusercontent.com/86272090/174970155-9eb2e488-7f27-4d55-a8e8-8779528a4071.png)
    - EKS 배포된 서비스 확인
    - ![image](https://user-images.githubusercontent.com/86272090/175186367-c9d5df98-1e0f-4112-ad41-bd4b71061598.png)



  - 분산 메시징 플랫폼 구성 
    - Kafka 설치
    - ![image](https://user-images.githubusercontent.com/86272090/175186158-6ace7af4-2c40-4158-b776-7da882f318a1.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175186224-2999e010-6589-4b00-b3c6-528e5fddd48c.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175186261-6bdb8a90-2a5b-44e9-b1bf-ec1c8a102ca9.png)
    - 상품등록 테스트
    - ![image](https://user-images.githubusercontent.com/86272090/175189037-1b6eeacf-0d93-4437-99f1-53e3979f51b2.png)
    - 주문생성 테스트
    - ![image](https://user-images.githubusercontent.com/86272090/175189102-49d436f9-18f8-4556-a104-526bcebca426.png)
    - 주문취소 테스트
    - ![image](https://user-images.githubusercontent.com/86272090/175189186-c8261a77-76df-438b-9ed2-64162108aa16.png)
    - Kafka 모니터링 결과
    - ![image](https://user-images.githubusercontent.com/86272090/175189317-102510ff-830a-498e-a48d-587e7e9d9b34.png)



  - SLA 운영 - 오토 스케일아웃(Auto Scale-out) 
    - Load Generator 생성
    - ![image](https://user-images.githubusercontent.com/86272090/175191425-8ebcd474-161f-4410-82b8-20d01efc49a7.png)
    - Load Generator 정상작동 확인
    - ![image](https://user-images.githubusercontent.com/86272090/175191586-79c8ebec-d66b-4d0f-9800-7261a83375c9.png)
    - HPA : 설정은 CPU 사용량이 15프로를 넘어서면 replica 를 10개까지 늘려준다
    - ![image](https://user-images.githubusercontent.com/86272090/175192600-8d8ae8f3-547e-46ce-bc5f-a803ddf80a6f.png)
    - Load Generator 발생
    - ![image](https://user-images.githubusercontent.com/86272090/175192676-00ebb4f8-239f-4d71-877b-28f8e03d152c.png)
    - HPA 확인
    - ![image](https://user-images.githubusercontent.com/86272090/175192720-529734e6-c587-4c25-905f-63d9294fd262.png)



  - SLA 운영 - 무정지 배포(Zero downtime Deploy) 
    - Readiness Probe 를 설정으로 새로 올려진 서비스의 Health Check 후 서비스 유입을 진행하여 무정지 배포 확인 
    - ![image](https://user-images.githubusercontent.com/86272090/175195339-7b742548-bebf-430f-b39d-a4acfa6490fe.png)



  - Service Mesh 인프라 구축
    - Istio 설치
    - ![image](https://user-images.githubusercontent.com/86272090/175209954-a96e0eb7-ca3f-4fac-beec-b5d154d2d8aa.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175210008-ab1591f1-435d-429e-9581-4175c03d4ded.png)
    - Istio 모니터링 툴(Telemetry Applications) 설치
    - ![image](https://user-images.githubusercontent.com/86272090/175210122-107d87d0-0e7e-4a5b-b0a2-2445232cca98.png)
    - 모니터링 시스템(kiali) 접속
    - ![image](https://user-images.githubusercontent.com/86272090/175210469-e4e29385-5f95-421e-9538-d31ba6d97ddf.png)
    - 분산추적 시스템(tracing) 접속
    - ![image](https://user-images.githubusercontent.com/86272090/175210784-b83f33b0-8a7d-44a9-88c6-b83407c7e33d.png)



  - Service Mesh 기반 마이크로서비스 Resilience 적용
    - ![image](https://user-images.githubusercontent.com/86272090/175215980-a605a4eb-c5c1-4908-9c8e-206cc8a429e3.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175216137-17b6a206-3b01-4660-a20e-1d5d1dea69b4.png)
    - 소스를 수정하여 재 배포
    - ![image](https://user-images.githubusercontent.com/86272090/175224982-9097db75-afb0-487a-b60a-eeebf358223b.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175225283-111c3568-ea6b-443f-a2a8-f0d211a7f8b6.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175863588-fdcbb19f-157b-404c-9ff0-fbf56a7070db.png)

  
  
  - 마이크로서비스 통합 모니터링
    - CNCF의 모니터링 스텍인 프로메테우스와 Grafana를 사용해 k8s 클러스터에 배포된 리소스를 모니터링 한다.
    - Prometheus / Grafana Monitoring Stack 설치
    - ![image](https://user-images.githubusercontent.com/86272090/175215723-3406eadb-f396-443b-b260-85054cbadaa1.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175219815-3f2bbf0a-d2e3-4b13-8a68-30265a0266a4.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175220360-006f16d3-8cee-4737-8e1b-8acd27ce94cf.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175226796-da699fd3-b64f-4a44-b375-27fdd852ec8f.png)

 

  - 마이크로서비스 통합 로깅
    - EFK(Elasticsearch, Fluentd, Kibana) 스텍을 클러스터에 설치하여 마이크로서비스 로그를 중앙에서 통합 모니터링한다.
    - 로그 수집기를 Fluentd 대신 동일 회사(Treasure Data)가 제작한 High Performance의 경량화 버전인 Fluent Bit를 적용한다.
    - 수집 데이터 저장소인 Elasticsearch를 기반으로 Kibana에서 시각화하여 통합 로깅한다.
    - Elasticsearch, Fluentd 설치
    - ![image](https://user-images.githubusercontent.com/86272090/175227401-f72118cc-adfb-4f26-8c49-a304e4a533ff.png)
    - 이슈노트
    - EFK 생성 시 Pod 생성 한계 오류 발생으로 모든 Pod 가 Terminating 현상 발생
    - 일부 서비스 Pod 정리하여 안정화 후 Worker Node 2기 증설 진행 및 서비스 재배포 진행
    - ![image](https://user-images.githubusercontent.com/86272090/175824389-93745b5b-9870-45e2-9e5d-5e90a2fbfb5f.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175824411-628cf5dd-4809-49f0-b020-21f6a48066aa.png)
    - 서비스 정상화 후 EFK 생성 시 Pod Pendig 오류 지속 발생하여 Worker Node 2기 추가 증설 진행하여 정상 생성
    - ![image](https://user-images.githubusercontent.com/86272090/175845311-61c1b4b2-74cf-442a-bd41-9a513f8d4b23.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175845189-7f9ac3c9-76f5-4865-bd2f-0bbaede24cd4.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175845231-2d313cf5-b0c4-4d61-a791-e7759d8c7475.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175845330-b15c7a3c-e312-4456-bbe1-c62a8a6ee5b4.png)
    - 설치확인
    - ![image](https://user-images.githubusercontent.com/86272090/175845806-934f6f4f-d899-45f3-8656-088897274a4b.png)
    - Fluent Bit 설치
    - ![image](https://user-images.githubusercontent.com/86272090/175847907-1af6cdda-d610-4770-a699-6b87fe5dc213.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175847952-6b164cf3-0a9e-41d7-ba11-866fc2abf8e5.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175847880-67637df9-4983-49a9-a7c1-952891525a4b.png)
    - 생성이 안되는 Worker Node 에서 Too many pods 발생되어 jaeger pod delete 하여 처리
    - ![image](https://user-images.githubusercontent.com/86272090/175848105-5515e4f3-0252-4bab-9b9c-cfba50af20cc.png)
    - 설치확인
    - ![image](https://user-images.githubusercontent.com/86272090/175848308-8d66b968-3edc-4815-9a93-ac2598ad1ac3.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175848419-db4d2234-b05c-461c-ab31-6fd6ef5ccc73.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175848530-9e035658-ae78-4bc0-b98e-4f1ce91ece0c.png)
    - kibana LB 생성
    - ![image](https://user-images.githubusercontent.com/86272090/175849522-a042d252-8dce-448b-b8c9-b18e3ec0791a.png)
    - Index 패턴 생성
    - ![image](https://user-images.githubusercontent.com/86272090/175849871-3c50b797-e99d-4a2e-b497-2310f1fa3f5c.png)
    - 로그 조회
    - ![image](https://user-images.githubusercontent.com/86272090/175850122-9e20b52f-0d6d-4153-99a2-b6405d590672.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175850396-fd792db5-b86d-4834-8887-673e4d5b558b.png)
 
 
 
  - 분산 메시징 플랫폼 모니터링
    - Kafka Dashboard 설치
    - ![image](https://user-images.githubusercontent.com/86272090/175859234-da98712c-2682-407f-92ef-526f8ab4e369.png)
    - 설치확인
    - ![image](https://user-images.githubusercontent.com/86272090/175859363-8801cfda-e799-46ac-90b2-140aafcd5753.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175859771-20ccf014-5bfb-4bfb-a38d-94fc42c3730a.png)
    - ![image](https://user-images.githubusercontent.com/86272090/175859894-2be060dd-107e-4e69-b515-888177f9f2c1.png)



