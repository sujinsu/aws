https://dumps.kr/dumps/amazon-saa-c02/2


1. EventBridge 가 최소 권한으로 람다 호출하기 위해 lambda:InvokeFunction에 대한 권한이 events.amazonaws.com에게 부여

2. 최대 절전모드 : 메모리를 보존하면서 비용 절감

3. CloudTrail 보호 예방 조치 및 보안 절차
- 유효성 검사 활성화
- AWS KMS 관리형 암호회 키(SSE-KMS) 와 함께 서버측 암호화를 사용하도록 여부 모니터링 AWS Config

4. 스토리지 성능 : 프로비저닝된 IOPS SSD(io1) 로 변경


5.  URL 쿼리문자열 기반 라우팅.  Amazon VPC 와 비즈니스 데이터센터간 VPN, ALB
- 하나의 ALB 두개의 대상그룹, 하나는 AWS 리소스 다른 하나는 온프레미스용. 호스트 추가, URL 쿼리기반 리스너규칙


6. 읽기 및 쓰기 트래픽 종종 예측 X, 스파이크 빠르게 발생
- 온디맨드 용량 모드. DynamoDB 테이블 생성


7. 전세계 컨텐츠 제공. ALB 뒤 프라이빗 서브넷 배포. 일부 국가 액세스 제한
- Amazon Cloudfront 사용. 차단된 국가 액세스 거부

8. 7개의 Amazon EC2 인스턴스. DNS쿼리가 모든 정상 EC2 인스턴스 IP주소 제공
- 다중값 라우팅 정책

9. 데이터 최소 5년 보관
- Amazon S3 standard 데이터 저장. 수명주기규칙 설정. 1년 후 S3 Glacier Deep Archive 로 전환. 5년후 데이터 삭제 수명주기규칙

10. Amazon RDS for PostgreSQL 데이터베이스 인스턴스 사용 웹서버 집합 관리. 1초 미만 RPO(복구 시점 목표)
- DB 인스턴스에 대한 다중 AZ배포 활성화


11. 조정 가능하고 일관된 IOPS 제공하는 데이터 스토리지
- 범용 SSD(gp2) 루트 볼륨 및 프로비저닝된 IOPS SSD(io1) 데이터 볼륨으로 EC2인스턴스 프로비저닝


12. IAM 규칙
- 다중 요소 인증(MFA) 활성화되지 않은 경우, 사용자는 s3:PutObject 를 제외한 모든 작업이 거부


13. 실시간 처리. Amazon S3 데이터 유지. 확장가능 내결함성 있는 아키텍쳐 설계
- DynamoDB 이벤트, 스트림 ->구문 분석. S3에 데이터쓰는 Lambda함수 트리거
- Amazon SNS 주제 이벤트 -> 구문 분석. S3에 데이터쓰는 Lambda함수 트리거


14. 일부는 조회수가 많고, 다른 사진은 적음. 최대 20MB 사진 업로드. 비용 효율적 액세스
- Amazon S3 intelligent-Tiering 스토리지 클래스에 사진 저장. 메타데이터와 해당 S3위치 DynamoDB에 저장


15. 정적 사진 저장 웹사이트
- Amazon S3 앞에 Amazon CloudFront 배포


16. 데이터베이스 계층 처리량 제공되는 DynamoDB사용. 트랜잭션볼륨 관리 X. 지연기간 문제.
- 플래시 판매 기간동안 DynamoDB 온디맨드 모드로 전환

17. 웹애플리케이션 호스팅. 비밀 미디어 파일 캐싱 시작. S3 버킷 자료 저장.  
- Amazon CloudFront 배포. S3 버킷 CloudFront 엣지 서버에 연결


18.웹 및 데이터베이스 구성 2계층 설계. 웹 서버 XSS(교차 사이트 스크립팅) 공격
- ALB 생성. 뒤에 웹계층 배치. AWS WAF 활성화 (Application layer 에 대한 공격. Shied Advanced L3/L4 공격 방어)


19. Amazon S3 민감데이터 저장. 기 사용 감사. 매년 키 교체
- 자동 교체 기능이 있는 AWS KMS(SSE-KMS) 고객 마스터키(CMK)를 사용한 서버측 암호화


20. 사용자 별로 분류된 AWS 청구 항목 요약 필요. 해당 보고서 데이터 얻는 방법
- 비용 탐색기에서 보고서 생성, 보고서 다운로드


21. 여러 파일. EC2 인스턴스에서 실행되는 여러 애플리케이션 서버는 파일 동시 액세스 가능 필요. 기본 제공 중복성
- Amazon Elastic File System(Amazon EFS)


22. 기밀 및 민감데이터에 대한 보안 액세스 제공. 온프레미스 Windows 파일 서버에 보관. 원격 트래픽 증가에 따라 파일 서버 용량 고갈
- 파일을 Windows 파일서버용 Amazon FSx 파일 시스템으로 마이그레이션. Amazon FSx 파일 시스템을 온프레미스 Active Directory 와 통합. AWS 클라이언트 VPN 구성 


23. 파일 공개적 액세스 가능. 누구든지 지정된 미래날짜 이전에 파일 수정 및 삭제 불가능. 안전한 솔루션
- S3 버전 관리가 활성화된 새 Amazon S3 버킷 생성. 지정된 날짜에 따라 보존기간이 있는 S3 Object Lock 사용. 
  정적 웹사이트 호스팅을 위해 S3 버킷 구성. 
  객체에 대한 읽기 전용 액세스 허용하도록 S3 버킷 정책 설정
  
  
24. 기업 10Gbps AWS Direct Connect 연결 온프레미스 서버 AWS 연결. 작업 부하 중요. 기존 연결 대역폭 최소화. 최대한 탄력적인 재해복구접근방식.
- 현재 리전과 다른 리전에 각각 하나씩 두 개의 새로운 Direct Connect 연결 설정

25. 두개의 가상 사설 클라우드 (VPC). 관리 VPC는 고객 게이트웨이를 통해 VPN 사용. 프로덕션 VPC
