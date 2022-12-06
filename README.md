# Overview

형상관리와 CI/CD 환경을 구성하게 되어 정리 차원에서 글을 작성합니다. 환경 구성에 사용된 도구들은 아래와 같습니다.

>
* nginx
* gitlab/gitlab-ee
* sonatype/nexus3
* minio/minio

모든 구성요소는 Docker 위에서 동작하도록 하고 Docker-compose를 통해 배포하도록 했습니다.
Docker와 Docker-compose의 용법은 [링크](https://docs.docker.com/get-started/)를 통해 확인하시면 좋겠습니다.

구성하고자 하는 환경을 간단하게 도식화 하면 아래와 같습니다.
![](https://velog.velcdn.com/images/jinwonseo/post/95a82a39-e26f-499d-98bb-c1b5123223d0/image.png)


# Reverse proxy

>
[Reverse proxy - wikipedia](https://en.wikipedia.org/wiki/Reverse_proxy)

Reverse proxy는 클라이언트의 요청을 받아 웹서버로 요청을 전달 하는 역할을 합니다. 지금의 구성은 하나의 서버에 여러 웹서비스들이 동작 하기 때문에 각 서비스는 포트를 분리해서 서비스 하게 됩니다. 이런 경우 domain.com:8080 과 같이 도메인 뒤에 포트를 명시해서 접속해야 하는 불편함이 있고 서버 측면에서도 여러 포트의 방화벽을 열어야 하는 문제가 있습니다. 이를 해소하기 위해 Reverse proxy를 구성하고 서브 도메인(sub.domain.com)을 이용하여 목적지를 구분하여 전달하도록 합니다. 서버는 80과 443포트만 열어두면 됩니다.

이 글에서는 Nginx를 활용하여 reverse proxy를 구성합니다.

# Sonatype Nexus3
>
[Sonatype repository management](https://www.sonatype.com/products/nexus-repository?topnav=true)

Nexus는 Self managed repository로 maven, npm, docker container image등의 사설 repository를 만들 수 있습니다. 

이 글에서는 maven repository와 docker container registry로 활용합니다.

# MinIO

>
[MinIO Multi-Cloud Object Storage](https://min.io/)

MinIO는 AWS S3와 호환되는 Object storage입니다. K8s, Docker, Linux, Windows, MAC 거의 모든 환경에 구성할 수 있습니다.

이 글에서는 Cache 서버로 MinIO를 사용하며 Docker container image로 설치합니다.

# Gitlab

> [Gitlab DevSecOps platform](https://about.gitlab.com/)

Gitlab은 형상관리 서비스로 CI, Registry, Wiki 등 많은 기능이 통합되어 있습니다. 일부 부가 기능은 구독이 필요하고 무료 CI shared runner의 사용 시간을 제공합니다. 이 시간을 넘기면 비용을 지불하거나 Self managed runner를 프로젝트에 등록하여 사용하면 됩니다. 설치형이고 오픈소스인 gitlab-ce도 존재하며 여러 리눅스 배포판을 지원하고 K8s와 Docker container image도 제공합니다.

이 글에서는 형상관리, CI, Registry로 Gitlab을 사용하며 Docker container image로 설치합니다.

# Nexts

지금 까지 언급한 서비스들을 구성하고 구성된 Gitlab에 Spring boot application을 올린 뒤 Gitlab의 Package registry, Container registry 그리고 Nexus 의 Maven repository, Container registry에 Gitlab CI를 통해 빌드/배포해 보도록 하겠습니다.

>
1. [Self managed repository and CI #1 - Overview](https://velog.io/@jinwonseo/self-managed-repository-and-ci-1)*ㆍ현재 글*
1. [Self managed repository and CI #2 - Docker network, Nginx reverse proxy](https://velog.io/@jinwonseo/self-managed-repository-and-ci-2)
1. [Self managed repository and CI #3 - Nexus3](https://velog.io/@jinwonseo/self-managed-repository-and-ci-3)
1. [Self managed repository and CI #4 - Minio](https://velog.io/@jinwonseo/self-managed-repository-and-ci-4)
1. [Self managed repository and CI #5 - Gitlab](https://velog.io/@jinwonseo/self-managed-repository-and-ci-5)
1. [Self managed repository and CI #6 - Gitlab-runner](https://velog.io/@jinwonseo/self-managed-repository-and-ci-6)
1. [Self managed repository and CI #7 - Gitlab-CI](https://velog.io/@jinwonseo/self-managed-repository-and-ci-7)

