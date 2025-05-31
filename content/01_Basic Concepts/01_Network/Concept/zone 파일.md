---
title: 네트워크) zone 파일
tags:
  - Network
  - FileSystem
aliases:
  - 영역파일
  - zone
---
연관: [[DNS]], [[00_BIND|BIND]]

# 영역 파일
> Zone File

[[DNS]] Zone 파일은 **DNS의 영역을 설정하는 텍스트 파일**이다. DNS 서버에서 사용하는 핵심 설정 파일들 중 하나로, 특정 도메인(zone)에 대한 DNS 레코드(주소, 별칭, 메일 서버 등)을 정리한 파일이다.

예를 들어 DNS는 도메인의 이름을 [[IP - Internet Protocol#IP의 주소|IP 주소]]로 변환하는데, 이 때 사용자가 만약 `example.com`을 질의했을 때 DNS 서버는 `zone`파일에 접근해서 `example.com`의 IP 주소 / 서브 도메인 등을 확인, 연결한다.

이처럼 `zone`파일은 DNS의 관리 정보가 집중된 파일이다.

---
## zone 파일의 예시

```zone
$TTL 86400
@   IN  SOA ns1.example.com. admin.example.com. (
        2023052501 ; Serial
        3600       ; Refresh
        1800       ; Retry
        604800     ; Expire
        86400 )    ; Minimum TTL

    IN  NS  ns1.example.com.
    IN  NS  ns2.example.com.

ns1 IN  A   192.0.2.1
ns2 IN  A   192.0.2.2

@   IN  A   192.0.2.100
www IN  CNAME   @
mail IN  MX  10 mail.example.com.
mail IN  A   192.0.2.200

```