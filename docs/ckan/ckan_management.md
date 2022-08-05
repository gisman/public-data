---
layout: default
title: CKAN 활용(관리자)
parent: CKAN
nav_order: 1
---

<span id="ckan-install"></span>
= CKAN install =

Python3 기반인 2.9를 설치.

Ubuntu 20.04 64 비트가 필요. Ubuntu 18.04 64 비트도 지원.

Ubuntu 20.04 에는 배포의 일부로 Python 3.8이 포함되어 있습니다.

Docker Compose는 잘 안 된다.

패키지 설치가 답이다: https://docs.ckan.org/en/2.9/maintaining/installing/index.html

<span id="ckan-패키지-설치"></span>
== CKAN 패키지 설치 ==

<span id="ckan에-필요한-ubuntu-패키지-및-ckan-확장을-설치할-수-있도록-git를-설치합니다"></span>
=== CKAN에 필요한 Ubuntu 패키지 (및 CKAN 확장을 설치할 수 있도록 'git')를 설치합니다. ===

<pre>sudo apt update
sudo apt install -y libpq5 redis-server nginx supervisor
sudo apt install -y git</pre>
<span id="ckan-패키지-다운로드-"></span>
=== CKAN 패키지 다운로드 : ===

<pre>cd ~/Downloads
wget http://packaging.ckan.org/python-ckan_2.9-py3-focal_amd64.deb</pre>
<span id="ckan-패키지-설치-"></span>
=== CKAN 패키지 설치 : ===

<pre>sudo apt install python3.8-distutils
sudo dpkg -i python-ckan_2.9-py3-focal_amd64.deb</pre>
<span id="postgresql-설치"></span>
== Postgresql 설치 ==

=== 설치 ===

<pre>sudo apt install -y postgresql</pre>
<span id="설치-확인"></span>
=== 설치 확인 ===

<pre>sudo -u postgres psql -l

                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | ko_KR.UTF-8 | ko_KR.UTF-8 | 
 template0 | postgres | UTF8     | ko_KR.UTF-8 | ko_KR.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | ko_KR.UTF-8 | ko_KR.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(3 rows)</pre>
<span id="사용자-생성"></span>
=== 사용자 생성 ===

암호 물어볼 때 입력

<pre>sudo -u postgres createuser -S -D -R -P ckan_default</pre>
<span id="db-생성"></span>
=== DB 생성 ===

<pre>sudo -u postgres createdb -O ckan_default ckan_default -E utf-8</pre>
<span id="db-접속-정보-수정"></span>
=== DB 접속 정보 수정 ===

CKAN 구성 파일 (/etc/ckan/default/ckan.ini) 파일 에서 sqlalchemy.url 옵션을 편집하고 올바른 비밀번호, 데이터베이스 및 데이터베이스 사용자를 설정하십시오.

<span id="solr-설치-및-구성"></span>
== Solr 설치 및 구성 ==

CKAN 매뉴얼의 solr-tomcat 대신 2021.01 최신 버전인 8.7 을 설치.

2021.02.02 현재 Solr 8.8.0

[[Solr]]는 [[jetty]]를 내장하고 있음.

최신 버전 장점

# 한글 검색 지원
# 보기 좋은 관리자 화면

<span id="java-install"></span>
=== java install ===

default-jre는 버전업되는데 2021.02.02 현재 openjdk 11.0.9.1 2020-11-04 설치 됨

<pre>sudo apt-get install -y default-jre </pre>
JAVA_HOME 설정. ~/.bashrc에도 추가

<pre>export JAVA_HOME=/usr/lib/jvm/default-java</pre>
<span id="solr-install"></span>
=== Solr install ===

<pre>cd ~/Downloads
wget https://downloads.apache.org/lucene/solr/8.8.0/solr-8.8.0.tgz</pre>
<pre>tar xvzf solr-8.8.0.tgz
sudo bash solr-8.8.0/bin/install_solr_service.sh solr-8.8.0.tgz</pre>
<span id="설치-테스트"></span>
=== 설치 테스트 ===

<pre>wget http://localhost:8983</pre>
<span id="core-생성"></span>
=== core 생성 ===

<pre>sudo su - solr
cd /opt/solr
bin/solr create -c ckan</pre>
<span id="core-설정"></span>
=== core 설정 ===

<pre>cd /var/solr/data/ckan/conf
cp solrconfig.xml solrconfig.xml.bak
vi solrconfig.xml</pre>
추가: <code>&lt;config&gt;</code> 바로 아래에

<pre>  &lt;schemaFactory class=&quot;ClassicIndexSchemaFactory&quot;/&gt;</pre>
변경: update.autoCreateFields를 false로.

<pre>  &lt;!-- The update.autoCreateFields property can be turned to false to disable schemaless mode --&gt;
  &lt;updateRequestProcessorChain name=&quot;add-unknown-fields-to-the-schema&quot; default=&quot;${update.autoCreateFields:true}&quot;
           processor=&quot;uuid,remove-blank,field-name-mutating,parse-boolean,parse-long,parse-double,parse-date,add-schema-fields&quot;&gt;
    &lt;processor class=&quot;solr.LogUpdateProcessorFactory&quot;/&gt;
    &lt;processor class=&quot;solr.DistributedUpdateProcessorFactory&quot;/&gt;
    &lt;processor class=&quot;solr.RunUpdateProcessorFactory&quot;/&gt;
  &lt;/updateRequestProcessorChain&gt;
을

  &lt;!-- The update.autoCreateFields property can be turned to false to disable schemaless mode --&gt;
  &lt;updateRequestProcessorChain name=&quot;add-unknown-fields-to-the-schema&quot; default=&quot;${update.autoCreateFields:true}&quot;
           processor=&quot;uuid,remove-blank,field-name-mutating,parse-boolean,parse-long,parse-double,parse-date&quot;&gt;
    &lt;processor class=&quot;solr.LogUpdateProcessorFactory&quot;/&gt;
    &lt;processor class=&quot;solr.DistributedUpdateProcessorFactory&quot;/&gt;
    &lt;processor class=&quot;solr.RunUpdateProcessorFactory&quot;/&gt;
  &lt;/updateRequestProcessorChain&gt;
으로</pre>
삭제

<pre>&lt;updateProcessor class=&quot;solr.AddSchemaFieldsUpdateProcessorFactory&quot; name=&quot;add-schema-fields&quot;&gt;
... 생략
&lt;updateProcessor/&gt;</pre>
변경

<pre>&lt;str name=&quot;hl.bs.language&quot;&gt;en&lt;/str&gt;
&lt;str name=&quot;hl.bs.country&quot;&gt;US&lt;/str&gt;
을

&lt;str name=&quot;hl.bs.language&quot;&gt;ko&lt;/str&gt;
&lt;str name=&quot;hl.bs.country&quot;&gt;KR&lt;/str&gt;
으로</pre>


<span id="schema-설정"></span>
=== schema 설정 ===

일반 user로.

<pre>cd /usr/lib/ckan/default/src/ckan/ckan/config/solr/
sudo cp schema.xml schema88.xml</pre>
schema88.xml 수정

<pre>&lt;defaultSearchField&gt;text&lt;/defaultSearchField&gt;
&lt;solrQueryParser defaultOperator=&quot;AND&quot;/&gt;
를

&lt;df&gt;text&lt;/df&gt;
&lt;solrQueryParser q.op=&quot;AND&quot;/&gt;
으로 변경</pre>
solrconfig.xml 수정. 위에서 q.op를 AND로 지정해도 ckan은 OR 검색을 한다. 8.8 버전은 solrconfig.xml을 수정해야 한다.

<pre>  &lt;requestHandler name=&quot;/select&quot; class=&quot;solr.SearchHandler&quot;&gt;
    &lt;!-- default values for query parameters can be specified, these
         will be overridden by parameters in the request
      --&gt;
    &lt;lst name=&quot;defaults&quot;&gt;
      &lt;str name=&quot;echoParams&quot;&gt;explicit&lt;/str&gt;
      &lt;int name=&quot;rows&quot;&gt;10&lt;/int&gt;
      &lt;str name=&quot;q.op&quot;&gt;AND&lt;/str&gt;   &lt;======= 추가</pre>
모든(5개) <code>&lt;tokenizer class=&quot;solr.WhitespaceTokenizerFactory&quot;/&gt;</code> 를 <code>&lt;tokenizer class=&quot;solr.KoreanTokenizerFactory&quot;/&gt;</code>로 변경

vi replace Tip: <code>:%s/WhitespaceTokenizerFactory/KoreanTokenizerFactory/g</code>

solr user로.

<pre>cd /var/solr/data/ckan/conf
ln -s /usr/lib/ckan/default/src/ckan/ckan/config/solr/schema88.xml schema.xml</pre>
<span id="solr-설정"></span>
=== solr 설정 ===

<pre>sudo vi /etc/default/solr.in.sh</pre>
ip 접근 제한

<pre>SOLR_IP_WHITELIST=127.0.0.1, 192.168.0.0/2</pre>
memory 설정

<pre>SOLR_HEAP=&quot;2g&quot;
SOLR_JAVA_MEM=&quot;-Xms2g -Xmx2g&quot;</pre>
<pre>sudo service solr restart</pre>
<span id="ckan-구성-파일에서-solr_url-설정을-변경"></span>
=== CKAN 구성 파일에서 solr_url 설정을 변경 ===

<code>sudo vi /etc/ckan/default/ckan.ini</code>

<pre>solr_url=http://127.0.0.1:8983/solr/ckan</pre>
<span id="ckan-구성-파일-설정"></span>
== CKAN 구성 파일 설정 ==

<code>sudo vi /etc/ckan/default/ckan.ini</code>

<pre>[app:main]
ckan.tracking_enabled = true</pre>
<pre>ckan.redis.url = redis://localhost:6379/0</pre>
<pre>ckan.site_url = http://192.168.0.101</pre>
<pre>ckan.auth.create_user_via_web = false</pre>
title 변경

<pre>ckan.site_title = CKAN2.9</pre>
<pre>ckan.storage_path = /var/lib/ckan
ckan.max_resource_size = 10
ckan.max_image_size = 2</pre>
<pre>ckan.datapusher.formats = csv xls xlsx tsv application/csv application/vnd.ms-excel application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
ckan.datapusher.url = http://127.0.0.1:8800/
ckan.datapusher.assume_task_stale_after = 3600</pre>
<pre>ckan.resource_proxy.max_file_size = 1048576
ckan.resource_proxy.chunk_size = 4096</pre>
<pre>ckan.storage_path = /var/lib/ckan/default</pre>
<pre>ckan.webassets.path = /var/lib/ckan/default/webassets</pre>

<span id="ckan-데이터베이스를-초기화"></span>
== CKAN 데이터베이스를 초기화 ==

<pre>sudo ckan db init</pre>
<span id="datastore-설정"></span>
== datastore 설정 ==

config 수정

<pre>ckan.plugins = datastore</pre>
<span id="사용자-및-데이터베이스-생성"></span>
=== 사용자 및 데이터베이스 생성 ===

<pre>sudo -u postgres createuser -S -D -R -P -l datastore_default

sudo -u postgres createdb -O ckan_default datastore_default -E utf-8</pre>
<span id="ckan-설정-변경"></span>
=== CKAN 설정 변경 ===

<code>sudo vi /etc/ckan/default/ckan.ini</code>

<pre>ckan.plugins = stats text_view image_view recline_view
을

ckan.plugins = datastore stats text_view image_view recline_view
으로</pre>
<pre>ckan.datastore.write_url = postgresql://ckan_default:pass@localhost/datastore_default
ckan.datastore.read_url = postgresql://datastore_default:pass@localhost/datastore_default</pre>
<span id="db-권한-설정"></span>
=== DB 권한 설정 ===

<pre>sudo ckan -c /etc/ckan/default/ckan.ini datastore set-permissions | sudo -u postgres psql --set ON_ERROR_STOP=1

sudo supervisorctl reload
sudo supervisorctl status</pre>
=== test ===

기본 get test

<pre>curl -X GET &quot;http://localhost/api/3/action/datastore_search?resource_id=_table_metadata&quot;</pre>
upload, delete test:

매뉴얼 참고 (https://docs.ckan.org/en/2.9/maintaining/datastore.html)

<span id="db-이관"></span>
== DB 이관 ==

<span id="db-추출"></span>
=== DB 추출 ===

<pre>sudo -u postgres pg_dump --format=custom -d ckan_default &gt; ckan.dump</pre>
<span id="db-load-및-solr-인덱스-생성"></span>
=== DB Load 및 solr 인덱스 생성 ===

사용자/암호까지 다 이관된다.

<s>단 API KEY는 이관되지 않는다.</s> 확실하지 않다.

<pre>sudo ckan -c /etc/ckan/default/ckan.ini db clean
sudo -u postgres pg_restore --clean --if-exists -d ckan_default &lt;ckan.dump
sudo ckan search-index rebuild</pre>

<span id="파일-업로드-설정"></span>
== 파일 업로드 설정 ==

이미지 파일은 업로드 할 수 있는데, 데이터스토어에 들어가는 형식의 파일은 제대로 업로드 안 된다.

<pre>sudo mkdir -p /var/lib/ckan/default
sudo chown www-data /var/lib/ckan/default
sudo chmod u+rwx /var/lib/ckan/default```</pre>
CKAN config file, after the [app:main]

<pre>ckan.storage_path = /var/lib/ckan/default</pre>
restart

<pre>sudo supervisorctl restart ckan-uwsgi:*</pre>
<span id="curl-test"></span>
=== curl Test ===

이미지 파일 업로드. format이 자동으로 처리 됨

<pre>curl -H'Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJONEtQMTZNVFZma3RjTC1LYlc4TlRiZkFFalQ4N1BwZDJKNGxVc29wUllCRXNZYUI3OE9XRUVCQ1JVQlgxeUR2TnNRdnNrUzJEWDhGZHlTTyIsImlhdCI6MTYxMTc4NzQ4NH0.BCd1fQJaMNWkX01Gwcdh_VF8LuhY5HUmaUyAKOsiTV0' 'http://localhost/api/action/resource_create' --form upload=@./logo.png --form package_id=data-seoul-go-kr-datalist-oa-10750-s-1 --form name=logo.png</pre>
CSV 파일 업로드. format을 지정해야 함

<pre>curl -H'Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJONEtQMTZNVFZma3RjTC1LYlc4TlRiZkFFalQ4N1BwZDJKNGxVc29wUllCRXNZYUI3OE9XRUVCQ1JVQlgxeUR2TnNRdnNrUzJEWDhGZHlTTyIsImlhdCI6MTYxMTc4NzQ4NH0.BCd1fQJaMNWkX01Gwcdh_VF8LuhY5HUmaUyAKOsiTV0' 'http://localhost/api/action/resource_create' --form upload=./@c.cvs --form package_id=data-seoul-go-kr-datalist-oa-10750-s-1 --form name=c.csv --form format=csv</pre>
[[분류:CKAN]]
