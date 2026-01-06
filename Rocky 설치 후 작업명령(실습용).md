### Rocky 설치 후 작업

#### `Activate the web console with: systemctl enable --now cockpit.socket` 메시지 없애기.
- rm -f /etc/issue.d/cockpit.issue
- rm -f /etc/motd.d/cockpit

#### virbr0 없애기
- systemctl disable --now libvirtd
- rm -f /etc/libvirt/qemu/networks/default.xml
- rm -f /var/lib/libvirt/network/default.xml

#### 방화벽 중지
- systemctl disable --now firewalld.service
- systemctl status  firewalld.service
