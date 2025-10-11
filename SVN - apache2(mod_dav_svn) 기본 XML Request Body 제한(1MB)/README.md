# SVN - apache2(mod_dav_svn) 기본 XML Request Body 제한(1MB)
사내 레거시 프로젝트 형상관리용으로 SVN 서버를 이용, 관리 중이다. 해당 서버는 apache2 WEB서버의 mod_dav_svn을 활용한다.

어느 날 다른 기능은 다 정상 동작 하지만, Jenkins에서 대형 프로젝트에 대한 커밋을 찍을 때 해당 에러가 발생하면서 Jenkins 빌드 Job이 실패하였다.
```text
[날짜] [core:error] [pid PID:tid TID] [client JenkinsIP] AH00539: XML request body is larger than the configured limit of 1000000
```

apache 전체에 기본 제한은 없으나, [**WebDAV 계열(XML 요청)에서는 기본 1MB 제한**](https://httpd.apache.org/docs/2.4/mod/core.html#limitxmlrequestbody)이 존재한다. 즉, 로그 내용 대로 commit 하려는 XML 요청이 1MB를 넘었고 기본 설정 제한에 걸린 것 (Jenkins가 전송한 commit 요청의 XML 바디가 1MB를 초과)

## 해결 방법
`mod_dav_svn` 설정 파일(`dav_svn.conf`)에서 요청 본문 크기 제한을 조정하였다.  
기본값 1MB → 10MB로 변경 후 apache2를 재시작하여 해결됨.
```text
...
<Location /SVN레포지토리 경로>
    # 추가
    LimitXMLRequestBody 10485760
</Location>
```