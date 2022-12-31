---
layout: post
date: 2021-12-13 22:05:37 +0900
title: '[misc] AWS 노트'
categories:
  - misc
tags:
  - misc
  - aws
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [AWS Documentation](https://docs.aws.amazon.com/)
- [AWS prescriptive Guidance](https://aws.amazon.com/ko/prescriptive-guidance/?nc1=h_ls&apg-all-cards.sort-by=item.additionalFields.sortText&apg-all-cards.sort-order=desc&awsf.apg-new-filter=*all&awsf.apg-content-type-filter=*all&awsf.apg-code-filter=*all&awsf.apg-category-filter=*all&awsf.apg-rtype-filter=*all&awsf.apg-isv-filter=*all&awsf.apg-product-filter=*all&awsf.apg-env-filter=*all)
- [AWS re:Post](https://www.repost.aws/)
- [AWS Support Center](https://console.aws.amazon.com/support)
- [AWS 지식 센터](https://aws.amazon.com/ko/premiumsupport/knowledge-center/)


## 참고

### 일반적인 설정 #1

- VPC와 서브넷으로 네트워크 구획 설정
- IAM으로 권한 제어
- EC2 인스턴스로 서버 컴퓨팅
- 타겟그룹(EC2)으로 인스턴스와 로드 밸런서(EC2)의 중간단계 설정
- 로드 밸런서로 네트워크 매핑 & 로드 밸런싱
- Route 53으로 도메인 생성하고 로드 밸런서로 연결
- EFS로 네트워크 스토리지 연결
- CodeDeploy와 S3로 배포 설정


## IAM, Identity and Access Management

카테고리: 보안, 자격 증명 및 규정 준수

사용자 관리와 동시에 AWS의 각 서비스들에 할당할 권한을 관리하는 서비스.

AWS는 루트 권한으로 직접 작업하지 말고 IAM을 통해 새 사용자를 추가한 뒤 작업할 것을 권장함. 방화벽과 관련 음슴.


## EC2, Elastic Compute Cloud

카테고리: 컴퓨팅

AWS를 대표하는 가상 컴퓨팅 서비스.

### Elastic IP addresses

elastic IP 주소를 관리하는 메뉴. elastic(탄력 있는) IP라곤 하지만 그냥 공인 IP 주소 관리 메뉴다. 

TODO 여기서 NAT 게이트웨이와 연결하는 머신가가 있는 것 같은데...?

### AMI, Amazon Machine Images

EC2 인스턴스의 파일들을 스냅샷으로 관리할 수 있게 해줌. 특정 시점의 스냅샷을 저장한 다음 이 이미지로 새 인스턴스를 만들 수 있다.

### Auto Scaling 오토 스케일링

EC2 인스턴스를 정해진 설정에 따라 자동으로 늘렸다 줄였다 해주는 서비스.

#### 오토 스케일링 적용 시 주의 사항

AWS 인스턴스의 기본 변수가 아닌 값을 식별자로 사용하는 환경을 구성하면 안됨.

뭔 소리냐면, 예를 들어 인스턴스 하나에 아파치 하나, 다른 인스턴스에 톰캣이 있다고 했을 때, 톰캣 쪽에 오토 스케일링을 적용해버리면 아파치-톰캣 통신에 필요한 AJP 포트번호가 오토 스케일링으로 늘어난 인스턴스에서 중복되니 잘못된 구성이다. (이러면 오토스케일링 때마다 일일이 변경해주거나 부팅 시 자동 실행되는 스크립트로 해결해야 한다)

따라서 EC2 인스턴스 하나에 아파치-톰캣을 짝으로(한 서버에 둘 다 넣는다는 말) 구성하는게 오토 스케일링을 위한 적절한 구성이라고 볼 수 있다.

### ELB, Elastic Load Balancing

말 그대로 로드 밸런싱 기능을 수행하는 서비스. EC2 대시보드에서도 연결되는데 '로드 밸런싱' 혹은 '로드 밸런서'로 찾을 수 있음.


## 코드 디플로이 CodeDeploy

카테고리: 개발자 도구

코드 배포 자동화 서비스. 패키징된 코드 적재, 스크립트 실행 등을 자동으로 수행한다. (빌드는 CodeBuild라고 따로 있음) S3나 EC2에 접근하게 하려면 IAM 할당이 필요한 모양이다.

코드 디플로이의 행동은 [appspec.yml](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html) 파일로 정의한다.

### 에이전트 설치

이 서비스가 작동하려면 EC2 인스턴스 혹은 온프레미스 장비에 코드 디플로이 에이전트가 설치돼 있어야 한다. 

가이드대로 아래처럼 실행해봤는데:

```bash
aws ssm send-command
    --document-name "AWS-ConfigureAWSPackage"
    --document-version "1"
    --targets '[{"Key":"InstanceIds","Values":[i-0fbcdc29af664625b]}]'
    --parameters '{"action":["Install"],"installationType":["Uninstall and reinstall"],"name":["AWSCodeDeployAgent"]}'
    --timeout-seconds 600
    --max-concurrency "50"
    --max-errors "0"
    --region ap-northeast-2
```

왜 때문인지 작동을 안해서 다른 방법을 찾은게 아래 내용이다(2022-07-02).

젠킨스로 배포할 EC2 인스턴스 터미널에서:

```bash
apt update
apt install ruby-full wget -y
```

설치 스크립트를 다운받아서 실행:

```bash
cd /home/ubuntu
wget 'https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install'
chmod +x ./install
./install auto
```

서비스 활성화 확인:

```bash
systemctl status codedeploy-agent.service
```

로그 확인:

```bash
journalctl -xfb -u codedeploy-agent.service
tail -f /var/log/aws/codedeploy-agent/codedeploy-agent.log
```

### 애플리케이션과 배포 그룹

전반적인 배포 방법과 대상을 설정할 수 있는 단위. 애플리케이션은 배포 그룹의 상위 개념으로 단순히 카테고리 이상의 역할은 없는 걸로 보임.

### 배포 구성(혹은 배포 설정)

여기서는 여러 인스턴스를 어떤 순서와 시간 간격을 두고 배포할 것인지를 결정한다.


## VPC, Virtual Private Cloud

카테고리: 네트워킹 및 콘텐츠 전송

가상 사설 클라우드. 공용 클라우드 환경에서 논리적으로 격리된 가상의 네트웤을 말함. 가령 EC2나 S3 인스턴스를 하나의 VPC로 묶으면 private IP로 서로 통신할 수 있다.

### NAT 게이트웨이

TODO 여기서 나가는 공인 IP를 결정한다는데? (탄력적 IP 주소가 그것)


## Route 53

카테고리: 네트워킹 및 콘텐츠 전송

~~라우터임 `¯\_(ツ)_/¯`~~ 도메인 등록, 트래픽 라우팅, 웹 앱 상태 확인 등의 기능을 제공하는 서비스다.

### 사설 DNS 서버

하위 메뉴 `호스팅 영역`에서 레코드를 private으로 생성하면 사설 DNS 서버가 됨. TODO 설명 추가

### dualstack이 뭐야

Route 53에서 레코드(= 대충 서브 도메인이라 생각하면 됨)를 생성할 때 `dualstack`으로 시작하는 도메인으로 라우팅하도록 지정할 수 있는데, dualstack이란 [EC2 로드 밸런서의 IPv4 혹은 IPv6 주소를 반환](https://stackoverflow.com/questions/41623388/what-does-the-dualstack-prefix-mean-in-aws-elb)하는 DNS 이름을 말한다.

예를 들어 `dualstack.noritersand.ap-northeast-2.elb.amazonaws.com`라는 dualstack DNS 이름은 `noritersand.ap-northeast-2.elb.amazonaws.com` DNS로 라우팅되며 EC2 서비스의 로드 밸런서 항목에서 찾을 수 있다.


## 람다 Lambda

카테고리: 컴퓨팅

실행할 코드를 작성하면 AWS가 알아서 인스턴스 만들어 실행해주는 서비스. 코드가 실행된 시간만큼 비용이 청구된다. 지원되는 언어는 Node.js, Python, Ruby, Java, Go, .NET

다들 많이 쓴다던데 아직 잘 몲겠다... 🤔


## S3

카테고리: 스토리지

클라우드 데이터 저장소. 서버 인스턴스를 버킷, 파일을 객체라고 부르는 모양.

끗.


## RDS

카테고리: 데이터베이스

말 그대로 관계형 데이터베이스를 제공하는 서비스.

데이터베이스 생성 시 Amazon Aurora 엔진의 serverless 유형을 선택할 수 있는데, 이렇게 하면 "사용자가 필요한 최소 및 최대 리소스를 지정하고, Aurora가 데이터베이스 부하에 따라 용량을 조정"한다.

### DB 인스턴스 터미널 접속 하기

일반적인 DBMS 툴을 통한 연결을 말하는 게 아니라, `mysql` 같은 명령어를... TODO


## CloudWatch

카테고리: 관리 및 거버넌스

모든 AWS 서비스에서 발생하는 이벤트나 상태, 로그를 확인할 수 있는 서비스.

### 로그 확인하기

하위메뉴인 `로그 > Logs Insights` 에서 로그 그룹을 선택한 뒤 쿼리를 실행하면 된다.

기본 쿼리는 이런 모양인데:

```bash
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

아래처럼 본인이 원하는대로 쿼리 작성하면 됨:

```bash
# 특정 도메인에 한해 필터링 + 원하는 필드 미리 노출
fields @timestamp, httpRequest.headers.15.value, @message
| filter (httpRequest.country='KR')
| filter (httpRequest.headers.0.value='fake.example.com')
| sort @timestamp desc
| limit 50
```


## 용어

### private subnet

VPC로 묶인 서버들의 가상 내부 통신망정도?

### ARNs, Amazon Resource Names

```
arn:partition:service:region:account-id:resource-id
arn:partition:service:region:account-id:resource-type/resource-id
arn:partition:service:region:account-id:resource-type:resource-id
```

AWS 리소스 고유 식별자. IAM, 람다, EC2, S3 등 AWS 서비스 내에서 생성한 리소스를 지정할 때 사용하는 값이다.

예를 들어 IAM 롤은 이렇게 생겼다:

```
arn:aws:iam::123456789012:role/codedeploy-service-role
```


## misc.

### AWS와 UFW

AWS에는 보안 그룹 설정이 따로 있고 여기서 인/아웃바운드 규칙을 설정할 수 있기 때문에 UFW(리눅스의 방화벽 시스템)를 따로 설정하지 않아도 된다.
