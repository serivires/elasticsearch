# elasticsearch

### 1. CentOS에서 yum으로 Elasticsearch & kibana 설치하기
1. PGP key 설치
```bash
$ sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```
2. RPM repository 설정
```bash
$ cd /etc/yum.repos.d/
$ sudo vi elasticsearch.repo
```
```
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```
3. 설치
```bash
$ sudo yum install elasticsearch
$ sudo yum install kibana
```
4. 시작과 종료
```bash
# init(centos6.x) or systemd(centos7.x)
$ ps -p 1
   PID TTY          TIME CMD
     1 ?        00:00:11 systemd
```
```bash
# centos7.x 부팅시 같이 시작
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable elasticsearch.service

$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable kibana.service

# centos7.x 수동으로 시작
$ sudo systemctl start elasticsearch.service
$ sudo systemctl stop elasticsearch.service

$ sudo systemctl start kibana.service
$ sudo systemctl stop kibana.service
```
```bash
# centos6.x 부팅시 같이 시작
$ sudo chkconfig --add elasticsearch

$ sudo chkconfig --add kibana

# centos6.x 수동으로 시작
$ sudo -i service elasticsearch start
$ sudo -i service elasticsearch stop

$ sudo -i service kibana start
$ sudo -i service kibana stop
```
5. log 파일 저장 위치
```bash
# elasticsearch
$ /var/log/elasticsearch/
```
6. 동작 확인
```bash
# elasticsearch
$ curl localhost:9200
{
  "name" : "2VDZPzQ",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "LD51FA6nSPCcT5bAIk_LoA",
  "version" : {
    "number" : "6.2.0",
    "build_hash" : "37cdac1",
    "build_date" : "2018-02-01T17:31:12.527918Z",
    "build_snapshot" : false,
    "lucene_version" : "7.2.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}

# kibana
```bash
$ curl localhost:5601
<script>var hashRoute = '/app/kibana';
var defaultRoute = '/app/kibana';

var hash = window.location.hash;
if (hash.length) {
  window.location = hashRoute + hash;
} else {
  window.location = defaultRoute;
}</script>
```
7. 시스템 설정 파일
    - [/etc/elasticsearch/elasticsearch.yml](https://www.elastic.co/guide/en/elasticsearch/reference/6.2/important-settings.html)
    - [/etc/kibana/kibana.yml](https://www.elastic.co/guide/en/kibana/6.2/settings.html)
8. 외부접속 허용
```
# ======================== Elasticsearch Configuration =========================
...
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: 0.0.0.0
#
# Set a custom port for HTTP:
#
http.port: 9200
#
# For more information, consult the network module documentation.
#
```
```
# Kibana is served by a back end server. This setting specifies the port to use.
server.port: 5601

# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: 0.0.0.0
```
