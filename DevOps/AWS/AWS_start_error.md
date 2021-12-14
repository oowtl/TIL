# AWS Error



## 현상

SSH 사용
Connection timed out 발생



## 해결

- `Mac aws ssh Operation timed out` 으로 구글 검색을 해보았다.
  보통 해결하는 방법으로 제시되는 것이 몇 개 있었다.
  - 연결을 차단하는 방화벽이 없습니다.
  - SSH 서비스가 인스턴스에서 실행 중입니다.
  - SSH TCP 포트 22가 수신 대기 상태에 있습니다.
  - [서버의 IP 주소 또는 호스트 이름](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html#using-instance-addressing-common)입니다.
  - [보안 그룹](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-security-groups.html#describing-security-group) 및 [네트워크 ACL](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#Rules)이 TCP 포트 22에서 수신되는 트래픽을 허용합니다.
- 요약하자면 SSH 를 인식할 수 있는 방법이 없다? 이렇게 볼 수 있는 것 같았다.
  vpc, subnet, igw 등이 설정이 되어있다고 보고 다른 게 있나 했는데 없었다.
- 처음부터 전부 새로 하니까 되었다. 즉, 무언가 연결이 되지 않았다는 것..!