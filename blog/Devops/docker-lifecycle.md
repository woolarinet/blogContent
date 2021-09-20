---
title: '컨테이너, 도커 라이프사이클'
category: 'Devops'
desc: 'Docker 및 컨테이너의 라이프사이클'
date: '2021-09-20'
thumb: 'devops'
keyword: 'Devops', 'MicroServices', 'MSA', 'Docker', 'Monolithic', 'lifecycle'
---

## :antigua_barbuda: Virtualized Resources
- Docker 컨테이너의 환경에 가장 많은 영향을 끼치는 두 자원은 네트워크와 파일 시스템이다.
  - `Network`: Docker 데몬은 여러 네트워크 인터페이스를 가질 수 있으며, 해당 인터페이스들의 호스트 네트워크 접근을 관리한다. 컨테이너들은 자신이 할당받은 네트워크 인터페이스만을 통해 내부 또는 외부와 통신할 수 있다. 격리된 네트워크는 외부로부터 접근을 적절히 차단한다면 각종 공격으로 컨테이너를 일차적으로 방어할 수 있는 역할이 된다.
    - 기본적으로 컨테이너들은 `bridge` 모드의 네트워크 인터페이스를 가진다.
      - 같은 네트워크 인터페이스 내의 컨테이너들과의 통신은 자유롭지만, 호스트 네트워크와의 연결은 허가된 포트를 제외하고 모두 차단한다.
    - `host` 모드는 같은 호스트와 컨테이너가 완전히 동일한 네트워크 인터페이스를 사용한다.
    - `none` 모드는 격리된 네트워크 영역을 갖으며 인터페이스가 없는 상태로 컨테이너가 생성되게 된다.
    - 이 밖에도 `overlay`, `container` 등이 있으며 통신 방식을 적절히 따져가며 사용하는 것이 좋다.
  - `Volume`: 파일시스템을 가상화한 개념이다. 목적지 없이 생성되면 다른 가상 머신의 가상 디스크처럼 활용되며, 호스트의 특정 파일 시스템에 마운트하여 사용할 수도 있다.
    - 컨테이너 내부에서 실행할 스크립트, 설정 파일 등은 컨테이너 내부로 복사되어 실행되기도 하지만 주로 `Volume`을 통해 전달 된다.
    - `Mysql 공식 이미지에서 제시하는 커스텀 설정파일을 볼륨을 통해 전달하는 명령어`
      - ``` bash
        $ docker run --name some-mysql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
        ```
# Container Lifecycle

![lifecycle.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Devops/docker-lifecycle/2.png)

- 생성, 수행, 임시중단, 종료, 강제종료 등의 상태가 있으며 ps 명령으로 목록을 확인할 수 있다.
- ps 결과가 늘어나는 것을 막으려면 종료된 컨테이너들을 rm으로 자주 지워주는 것이 좋다.
- ``` bash
  $ docker ps # shows running containers
  $ docker ps --all # shows created, running, stopped, ... containers
  $ docker run python:3.9 python -c 'print("hello world!")' # starts running container
  $ docker rm great_bassi # remove exited container
  ```

# Docker Lifecycle

![lifecycle.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Devops/docker-lifecycle/1.png)

- `Registry`로 부터 `Image`를 Pulling하여 다운 받는다.
  - Pushing으로 `Image`를 `Registry`에 저장을 할 수도 있다.
- Pulling된 `Image`를 실행하기 위해 Create 명령어를 사용하여 `Container`를 만들어준다.
- Start 명령어를 통해 `Container`를 메모리에 띄워 어플리케이션을 동작시켜 줄 수 있다.
- `Container`를 삭제시켜주기 위해서는 rm 명령어를 통해 삭제할 수 있고, `Image`를 삭제시켜주기 위해서는 rmi 명령어를 사용할 수 있다.

## References
- [Kubernetes]
- [appleseed.dev]
- [Inflearn-Lecture]

[Kubernetes]: https://Kubernetes.io/docs/concepts/overview/what-is-Kubernetes/

[appleseed.dev]: https://blog.appleseed.dev/post/docker-from-basics-to-production/

[Inflearn-Lecture]: https://www.inflearn.com/course/%EB%8D%B0%EB%B8%8C%EC%98%B5%EC%8A%A4-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EB%A7%88%EC%8A%A4%ED%84%B0