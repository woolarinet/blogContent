---
title: '도커와 마이크로서비스 이해'
category: 'Devops'
desc: 'Docker와 MicroService에 대한 얕은 이해'
date: '2021-09-13'
thumb: 'devops'
keyword: 'Devops', 'MicroServices', 'MSA', 'Docker', 'Monolithic'
---

## Monolithic Architecture
- 전통적인 아키텍처, 기존에 사용하던 서비스 방법
- 서비스가 하나의 애플리케이션으로 돌아가는 구조
- 하나의 어플리 케이션이 거대한 아키텍처를 이루고있다.
### 단점
- 기존의 애플리케이션을 그대로 복제하여 로드밸런싱을 하기에 불필요한 서비스까지 모두 복제된다.
- 각각의 기능들은 서로 다른 기능을 제공하여 버전의 종속성을 필요한 경우가 존재하여 관리하기가 매우 어렵다.
- 약간의 수정에도 전체 빌드 및 배포가 필요하다.
  - 소스코드 전체가 하나로써 동작하기 때문.
  - 프로그램의 크기가 커질 수록 전체 테스트를 하는데 시간이 오래 소요하게 된다.
  - 하루에 버그가 여러개 발견되는 경우엔 대응하기가 상당히 까다롭다.

## MicroServices란?
- 모놀리식 아키텍처의 대안으로, 반대되는 개념이다.
- 애플리케이션의 각 기능을 분리하여 개발 및 관리를 할 수 있다.
### 장점
- 서비스 단위의 빠른 개발이 가능하다. 개발자가 특정 비즈니스 로직에 대해서만 집중하여 개발 가능
- 고효율 저비용 Scale-Out 구조가 된다.
  - 서비스 단위로 스케일링이 가능해 불필요한 서비스를 줄이고 더 많은 자원이 서비스의 확장이 가능해진다.

    ![msa.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Devops/msa_docker/1.png)
**분산 시스템 환경에서 트랜잭션을 보장하거나 테스트, 배포, 관리의 복잡함이 있다.**

# 도커와 마이크로 서비스의 이해
`'컨테이너'`
  - 용기와 같다고 이해할 수 있다.
  - 환경을 격리시키는 역할을 하며, 가상머신을 사용해 각 마이크로 서비스를 격리하는 기술이다.
  - 하드웨어를 전부 구현하지 않기에 매우 빠른 실행이 가능하다.
  - 프로세스의 문제가 발생할 경우 컨테이너 전체를 조정해야 하기 때문에 컨테이너에 하나의 프로세스를 실행하도록 하는 것이 좋다. (== 브라우저)
  - 커널의 기능을 사용하여 네임스페이스와, 컨트롤그룹을 이용하여 컨테이너를 격리시킬 수 있다.
    - `Linux Name Space`: 프로세스마다 파일시스템을 따로보게 하고, 네트워크, 호스트 등을 따로 따로 이용하게 한다.
    - `Control Group`: 프로세스로 소비하는 리소스의 양을 제한한다. (CPU, 메모리, I/O, 네트워크 대역 등)

  ![msa.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Devops/msa_docker/2.png)

`Docker`
  - 컨테이너 기술을 지원하는 다양한 프로젝트 중 하나이다.
  - 현재 컨테이너 기술의 사실상 표준이다.
  - 다양한 운영체제에서 사용 가능하며 애플리케이션에 국한되지 않고, 의존성 및 파일 시스템까지 패키징하여 빌드, 배포, 실행을 단순화 할 수 있다.
  - 위에서 설명한, 네임스페이스와 Cgroup과 같은 리눅스의 커널 기능을 사용하여 가상화한다.
  - 다양한 클라우드 서비스 모델과 같이 사용 가능하다. (IaaS, PaaS, SaaS)
    - `이미지`: 필요한 프로그램과 라이브러리, 소스를 설취한 뒤 만든 하나의 파일

    ![msa.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Devops/msa_docker/3.png)

    ### 도커 아키텍처
    - Docker Engine: 이미지, 네트워크 등의 관리 역할
    - Containerd: Container를 관리해주는 Daemon
    - 위 두개가 각각 돌아가기 때문에 Docker Engine을 재시작해도 이미지에 영향이 없다.
    ### 도커의 한계
    - 서비스가 커질수록 관리해야 하는 컨테이너의 양이 상당히 증가한다.
    - 도커를 사용하여 관리해도 쉽지않은 형태이다.
    - 이를 해결하기 위한 오픈소스로 쿠버네티스가 등장하였다. 이 부분은 따로 정리를 할 예정 :)

## References
- [Kubernetes]
- [boanproject]

[Kubernetes]: https://Kubernetes.io/docs/concepts/overview/what-is-Kubernetes/

[boanproject]: https://www.boanproject.com