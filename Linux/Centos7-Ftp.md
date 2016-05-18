# Centos7 Ftp Setting

참고 : http://bj001kim.blogspot.kr/2015/05/centos-7-ftp.html

#### 1. vsftpd 설치
   $ yum -y install vsftpd
   $ systemctl restart vsftpd
   $ systemctl enable vsftpd

#### 2. vsftpd.conf 파일 설정.
 $ vi /etc/vsftpd/vsftpd.conf 오픈.

  ## 아래내용으로 설정을 한다. ##
   - anonymous_enable=NO
   - local_enable=YES
   - write_enable=YES
   - chroot_local_user=YES
   - chroot_list_enable=YES

#### 3. chroot_list_enable설정을 YES로 한경우 chroot_list 파일을 작성한다.
   $ vi /etc/vsftpd/chroot_list 생성. ( 계정을 추가한다. )

#### 4. 방화벽 설정.
   $ firewall-cmd --permanent --add-port=21/tcp
   $ firewall-cmd --permanent --add-service=ftp
   $ firewall-cmd --reload
   $ setsebool -P ftp_home_dir on

#### 5. vsftpd 재구동
   $ systemctl restart vsftpd


#### 이슈1) 파일업로드 시 553 Could not create file 오류
      -> 파일의 소유자 및 소유그룹에 제한되는 현상.

#### 해결) http://cheolgoon.tistory.com/84
      -> $ chown 사용자:그룹 디렉토리명

#### 이슈2) 디렉토리 목록 불러오지 못하는 경우 ( 226 transfer done( but failed to open directory)
      -> selinux 기능으로 인한( selinux란 프로세스의 접근제어명령에 대해서 억제, 기록, 탐지에 필요한 기능 )

#### 해결) http://blog.danielburrowes.com/2009/06/vsftpd-transfer-done-but-failed-to-open.html
      -> $ vi /etc/selinux 오픈
      -> SELINUX=disabled로 수정.
