# 문재인 대통령 취임 후 국민들의 북한, 통일에 대한 인식 변화

## - Hadoop 기반의 빅데이터 NLP 시스템 개발

### 목차

1. [시스템 아키텍처](#시스템-아키텍처)
5. [데이터 분석 결과](#데이터-분석-결과)
6. [Todo List](#Todo-List)

<br/>

### 시스템 아키텍처

![](./screenshots/system_architecture.png)

<br/>

### 데이터 분석 결과

- Visualization with zeppelin notbook (.gif)

![](./screenshots/zeppelin.gif)

<br/>

- 결과

  - '북한', '통일' 언급 수 증가

    - '북한' 언급량 그래프 : 2017년에 비해 2018년 '북한' 언급량 증가![](./screenshots/result1-1.png)

    - '통일' 언급량 그래프 : 2017년에 비해 2018년 '통일' 언급량 증가

      ![](./screenshots/result1-2.png)

  - 긍, 부정 추이 변화

    - '북한' 긍, 부정 추이 그래프 : 2017년에는 전반적으로 부정적이었지만 2018년은 부정보다 긍정 비율이 높은 것을 확인할 수 있다. 
      ![](./screenshots/result2-1.png)

    - '통일' 긍, 부정 추이 그래프 : 전반적으로 부정이 긍정보다 높지만 2018년에는 긍, 부정 차이가 감소한 것을 확인할 수 있다.

      ![](./screenshots/result2-2.png)

  - '종전'

    가장 많이 언급된 15개의 키워드 그래프에서 2017년에는 '종전'이란 단어가 등장하지 않았다.

    2018년 3월 9일 처음 등장하여 4월 17, 18, 26, 27일(1차 남북정상회담), 9월 19일(3차 정상회담)에 나타나게 된다. 

    3월 9일 179번 언급하였고 특히 1차 남북정상회담(4월 27일)에는 1,224번 언급으로 정점을 찍게 된다

    - 3월 9일 - 179번

      <img src="./screenshots/result3-1.png" width="370" height="350"/>

    - 4월 27일 - 1,224번

      <img src="./screenshots/result3-2.png" width="430" height="350"/>





### Todo List

**2018-11-04**

- [x] tweet scraper 찾기 (twint, twitter-scraper)
- [x] 데이터 수집..

> :memo:
>
> 트위터 api 이용하는 것은(tweepy) 7일 이내 데이터만 가능하고 이전 자료를 보려면 돈을 내야됨 -> 웹에서 긁어모으자...
>
> 라이브러리 twint, twitter-scraper.. firefox 무한스크롤 이용해서 직접……
>
> twitter-scraper는 25페이지 정도까지만 보장 가능(486트윗) -> twint 사용
>
> python2.* => `$ python` python3.* => `$ py`
>
> :bug:
>
> `Command "/Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7 -u -c "import setuptools, tokenize;__file__='/private/tmp/pip-install-pdut0psv/cchardet/setup.py';`..-> twint install이 안됨.  이것저것 하다가 python 요구버전이 3.6이라 윈도우로 옮겨서 3.6.7깔았더니 해결...
>
> 실행했더니 => `ModuleNotFoundError: No module named 'aiohttp_socks'` -> twint uninstall하고 `pip3 install --upgrade -e git+https://github.com/twintproject/twint.git@origin/master#egg=twint`                    이렇게 설치했더니 해결
>
> <br/>
>
> 이렇게 쉽게 해도 되는 걸까….ㅠ… 바로 디비에 저장해야되나..? 

<br/>

##### 2018-11-06

- [x] twint 에러 해결
- [x] vscode 연결
- [ ] 데이터 파일 read 
- [x] nifi 설치 —> 했는데 hortonworks로 다시 깔기 

> :memo:
>
> twint 정지계정트윗나면 에러남 -> output에 에러처리해주기(나중에 올려줘야지..)
>
> <br/>
>
> vm과 vscode 연결
>
> 1. vscode에서 extentsions 'Remote VSCode' 설치
>
> 2. rmate 설치
>
>    ```
>    wget https://raw.githubusercontent.com/sclukey/rmate-python/master/bin/rmate
>    chmod +x ./rmate
>    sudo mv ./rmate /usr/local/bin/rmate 
>    ```
>
> 3. `$ ssh -R 52698:localhost:52698 maria_dev@localhost -p 2222`
>
> 4. `$ rmate project/tw.py`
>
> :bug:
>
> hdfs에 test파일 올렸는데 한글 다 깨짐 -> `$ echo $LANG` `$ locale` 보면 제대로(ko_KR.UTF-8) 되어있는데ㅠ
>
> df로 만들면 스키마가 이상하게 c1, c2… 이렇게 됨…..  ---> csv load하면서 `header="true"` 빼먹음
>
> `UnicodeEncodeError: 'ascii' codec can't encode characters in position 1551-1552: ordinal not in range(128)` 파일 불러올 때 인코딩 설정하는데도 왜이러지...

<br/>

##### 18-11-10

- [x] 데이터 파일 read -> 인코딩

>:memo:
>
>nifi 실행 : `./bin/nifi.sh start` -> 포기^^;;
>
>:bug:
>
>여전히 한글 인코딩… `hadoop fs -text data/tweet_test.csv` 하면 잘보임… 테스트용 만들어보았는데 여전히 똑같... `df.show()`해서 안나오던 것이.. `print(df)` 하니깐 나옴...^^...
>
>```python
>print(sys.stdout.encoding)
>print(sys.stdout.isatty())
>print(locale.getpreferredencoding())
>print(sys.getfilesystemencoding())
>```
>
>이제 출력은 되는데.. u"\ub098\ub3c4 \uc5ec\uae30\uc11c \uc774 \uc9c0\ub784\ub4e4 \ud558\uace0\uc788\uc9c0\ub9cc... \ubaa8\ub450 이렇게 출력됨..

<br/>

##### 18-11-12

- [ ] nifi 설치
- [ ] virtual box git..... -> 이미 있음..ㅎ
- [x] date time 합치기
- [x] 시간 조정
- [x] 필요없는 열 삭제
- [x] konlp 명사 나누기

> :memo: 
>
> 다시 nifi 도전해보자..! hdf 설치
>
> :bug:
>
> `mount: unknown filesystem type 'vboxsf'` -> VBoxGuestAdditions 설치(버전 맞게)
>
> 실행x -> `sudo yum install gcc kernel-devel make bzip2 ` -> `VBoxLinuxAdditions.run` 실행
>
> `Please install the Linux kernel "header" files matching the current kernel`
>
> `mount: only root can use "--types" option`
>
> <br/>
>
> `sys:1: DtypeWarning: Columns (0,1,2,6) have mixed types. Specify dtype option on import or set low_memory=False.` -> read_csv에서 dtype 설정
>
> date 변경하는데 안됨 -> date에 이상한 주소가 들어가 있음.. `errors='coerce'` 추가
>
> `AttributeError: type object 'datetime.datetime' has no attribute 'timedelta'` -> `from datetime import datetime` 을 `import datetime` 으로 변경
>
> konlp 설치 중 `error: command 'gcc' failed with exit status 1` -> `xcode-select --install`
>
> 만약 `xcode-select: command not found` 라고 뜨면 직접 apple developers에서 command line tools다운
>
> `RuntimeError: No matching overloads found for simplePos09 in find` -> string으로 타입 바꿔줌

<br/>

##### 18-11-13

- [ ] nlp자른 것 df 저장
- [ ] nifi설치

> :memo:
>
> 한나눔이 다른 것보다 외래어, 영어, 한자 잘 잡아냄.
>
> :bug:
>
> `ValueError: Length of values does not match length of index` -> 한번에 전체로 나와서 따로 따로 
>
> 리스트 df에 저장이 안됨...
>
> `ImportError: No module named ambari_commons.exceptions`
>
> ambari 이상해져서 가상머신 새로 했더니 `unable to sign in. invalid username/password combination.` admin계정 로그인 안됨 -> `# ambari-admin-password-reset` 
>
> 엄청난 삽질 끝에.. `# ambari-server setup` 
>
> `# ambari-server install-mpack --mpack=http://public-repo-1.hortonworks.com/HDF/centos7/3.x/updates/3.2.0.0/tars/hdf_ambari_mp/hdf-ambari-mpack-3.2.0.0-520.tar.gz --verbose` 
>
> https://docs.hortonworks.com/HDPDocuments/HDF3/HDF-3.2.0/release-notes/content/hdf_repository_locations.html
>
> ambari에서 nifi 추가하는데 설치 안됨 -> 버전 안맞았음ㅠ 현 ambari 버전 2.6.2 최소 2.7 이어야됨.
>
> https://supportmatrix.hortonworks.com/
>
> https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.0.0/bk_ambari-upgrade/content/upgrade_ambari.html < 관련 문서. 내일 해보자

<br/>

##### 18-11-14

- [x] ambari update

>:bug:
>
>`ImportError: No module named ambari_commons.exceptions` -> 앞에 sudo
>
>nifi ui가 실행 안됨..9090포트 -> /private/etc/hosts에서 `127.0.0.1 localhost sandbox.hortonworks.com sandbox-hdp.hortonworks.com sandbox-hdf.hortonworks.com` 추가! -> 안됨ㅠ
>
>`Permission denied: 'conf/bootstrap.conf'` -> 루트계정으로
>
>가정 1. 포트번호
>
>2. 로컬호스트
>3. admin 권한

<br/>

##### 18-11-15

> hive 이상해진게 버전 문제 인 것 같다.. HDP버전 생각 못함ㅠ마지막으로 다시 삭제설치해보기..^^;;;
>
> 버전 맞는데도 안됨.. -> hive nifi 충돌인가 ㅠㅠ
>
> https://supportmatrix.hortonworks.com/

<br/>

##### 18-11-16

- [x] 명사로 나눈 것 str | 로 나눈 것으로 변환
- [x] 중복 행 제거
- [ ] 이상치 처리
- [x] hashtag 분리
- [x] mention 분리

> :memo:
>
> iterrows()보다 itertuples()이 훨씬 빠름
>
> df 값 그냥 update하면 에러남 -> index로 at[] 이용하여 값 변경하기
>
> 도배하는 트윗들 없애버릴까?...
>
> :bug:
>
> `ModuleNotFoundError: No module named 'NumPy'` -> numpy 소문자로 쓰니 해결

<br/>

##### 18-11-20

- [x] scraper 날짜 유동적이도록 고치기 + 파일 삭제
- [ ] oozie python shell

> :memo:
>
> python scraper oozie에 올리기
>
> `hdfs fs -put {vm} {hdfs}`
>
> :bug:
>
> 터미널 시간 이상하게 나옴 -> `sudo date {month}{day}{hour}{minute}{year} ` ex) 2018년 11월 20일 18시 24분 -> `sudo date 1120182418`
>
> twitter가 계정마다 시간설정 이상하게 되어있음 -> twitter 로그인 -> 설정 에서 고쳐주면 됨. GMT+9(csv는 utc시간)
>
> `/usr/bin/env: python3: No such file or directory` -> oozie에 python3 설치해야되는듯... 이미 설치되어있음.

<br/>

##### 18-11-21

> :memo:
>
> 긍부정 학습할 데이터 http://word.snu.ac.kr/kosac/lexicon.php 에서 얻음. > http://word.snu.ac.kr/kosac/pub/PACLIC26.pdf
>
> https://docs.google.com/spreadsheets/d/1OGAjUvalBuX-oZvZ_-9tEfYD2gQe7hTGsgUpiiBSXI8/edit#gid=0 -> KoNLPy tag chart -> 위 학습데이터는 Komoran과 가장 비슷한듯
>
> :bug:
>
> `ImportError: No module named numpy` -> `sudo pip install numpy ` `sudo pip install --trusted-host pypi.python.org --trusted-host files.pythonhosted.org --trusted-host pypi.org numpy`-> RegressionEvaluator import에서 위 에러 남. 아직 해결x

<br/>

##### 18-11-22

- [x] 형태소 나눈 것 ;/로 바꿈
- [x] 학습데이터 만들 것 처리 : 형태소 분리, 감정 사전 읽어와서 비교 

>다 스파크로 전처리 해야할 듯... -> 시간이 있을까???ㅠ 다른 것부터하고 시간 남으면 바꾸자..
>
>스크래퍼.. 굳이 hdfs에서 파일 삭제할 필요가 있을까? -> 일단 주석처리
>
>감정 분석 -> 감정 사전 밖에 없음.. -> 이것 이용해서 내가 학습데이터 만들자
>
>:bug:
>
>`[UnicodeEncodeError: 'ascii' codec can't encode character](https://stackoverflow.com/questions/39662384/pyspark-unicodeencodeerror-ascii-codec-cant-encode-character)` -> spark에서 show할 때마다 나는 에러 => spark run하기 전에 `$ export PYTHONIOENCODING=utf8` 입력하면 됨!
>
>oozie... running 8시간째…… 
>
>다시 설치했더니 카산드라 안됨... 마지막으로 또 삭제해보자..^... -> 역시 삭제는 진리다

<br/>

##### 18-11-24

> :memo:
>
> python 실행할 때 맨 위 python argument에 파일 이름!!!! 그리고 file에 해당 파일 넣기!!! 며칠동안삽질이엇나ㅠ
>
> python3.6으로 입력하면 3버전도 됨!
>
> :bug:
>
> oozie 왜 echo도 안되니,,,,,,
>
> `Failing Oozie Launcher, Main class [org.apache.oozie.action.hadoop.ShellMain]`
>

<br/>

##### 18-11-25

- [x] 감정분석시 중복점수되는 것 해결
- [ ] spark noun 나눠서 top 20 뽑기
- [x] python으로 전처리한 것 => 스파크로 바꾸기
- [x] twitter로 tokenizer 바꾸기
- [x] csv 저장
- [x] hbase 저장

> :memo:
>
> 감정사전 자료가 pos(약 만이천개), neg(약 사천개)라서 항상 긍정으로 나오는데 어떡하냐……... 너무 압도적이라서 %도 안됨..
>
> 스파크는 tokenizer가 없고(있는줄알았는데 pyspark가아니였음..)... python은 oozie에서 하자니... gcc가 안되고......
>
> :bug:
>
> `xcode-select: command not found` 여기서는 xcode CLT 못설치하는데 그럼 로컬에서 해야되는건가....ㅠ -> yum으로 gcc 설치 가능
>
> `UnicodeDecodeError: 'ascii' codec can't decode byte 0xc2 in position 371: ordinal not in range(128)` -> `with open("./data/result.csv", "r", encoding="utf-8")` 

<br/>

##### 18-11-26

- [x] date 추출되는지 확인할 것!
- [x] python -> vm에서 
- [ ] hbase 저장할 column 지정(날짜를 맨 앞에 리트윗수, word, @, 해시태그...)
- [x] python module묶어서 oozie 

> :bug:
>
> `SyntaxError: Non-ASCII character '\xec' in file variable.py on line 1,` -> python2버전은 맨 위에 인코딩..
>
> python2버전에서는 gcc jpype 잘 됨
>
> `UnicodeDecodeError: 'ascii' codec can't decode byte 0xea in position 0: ordinal not in range(128)` -> 
>
> ```python
> #-*- coding: utf-8 -*-
> import sys
> reload(sys)
> sys.setdefaultencoding('utf-8')
> ```
>
> http://www.adaltas.com/en/2018/03/06/execute-python-in-an-oozie-workflow/  -> oozie python module
>
> `ImportError: Missing required dependencies ['numpy']` pandas.. 동적import를 바꿔주니 해결
>
> `ImportError: cannot import name 'multiarray'`

<br/>

##### 18-11-27

- [x] 스파크 인코딩 또는 다른 것으로 바꿀 것..

> :memo:
>
> hive로 바꿀까...ㅎ
>
> :bug:
>
> hive로 했는데도 ??뜸.. 인코딩해도... -> 스파크 sql은 쓰지말고..

<br/>

##### 18-11-28

> :memo:
>
> 단어 빈도수 분석했는데 쓸모없는 데이터 너무 많음 -> 명사 중에서도 보통명사, 고유명사, 수사 만 추출(수사도 뺄까..) -> 그래도 쓰레기값많음ㅠㅠㅠㅠ 하지만 22가 최대치
>
> 영어 추출하려면 nlp 써야하는 거니..? -> 외국어 태그 F였음... 하지만..
>
> 외부링크 삭제
>
> 문장부호 추출
>
> 맞춤법, 띄어쓰기가 잘안되어있어서 값이 잘 안나오는 것 같다 -> 테스트해보니 잘 되는 라이브러리 없음ㅠㅠ흑
>
> :bug:
>
> 맞춤법 검사하는데 `ValueError: No JSON object could be decoded`
>
> 라이브러리 맨날 고쳐쓰는듯.. py_hanspell에서 baseurl, req import 바꿔야됨

<br/>

##### 18-11-29

> :memo:
>
> 불용어 데이터
>
> https://github.com/6/stopwords-json 
>
> :bug:
>
> `xml.etree.ElementTree.ParseError: not well-formed (invalid token) in Python` -> string에 &이 있어서 발생한 오류. 구두점, 특문 다 제거해야겠음.
>
> 불용어확인하니깐..뭔가 이상해짐

<br/>

##### 18-11-30 & 18-12-01

- [x] sentiment 형태소 별로 분리
- [x] sentiment dict로 만들기(형태소별로?)
- [x] 다시 용언도 넣기
- [x] date별 가장 많은 15개 단어 추출

> 불용어를 konlpy 전에 하니깐 문장이 이상해짐 -> 자르고 불용어 삭제하자
>
> 분석 : date count null 삭제
>
> :bug:
>
> 트위터에서 곷 이런 단어를 인코딩 못함.. -> `str(hangul_text).decode('utf-8', errors="replace")`

> https://konlpy-ko.readthedocs.io/ko/v0.4.3/examples/wordcloud/ 
>
> http://word.snu.ac.kr/kosac/lexicon.php

<br/>

##### 18-12-02

- [x] oozie scraper
- [x] oozie local to hdfs
- [x] sentiment 함수 (아직 확인 못함)

> dict keyerror나는 게 타입이 안맞아서 그런 것이 아니라 해당 키가 없어서였다..
>
> hive
>
> oozie는 루트계정에서!
>
> :bug:
>
> `sudo: no tty present and no askpass program specified` -> oozie에서 sudo했을 때 비밀번호를 알지 못해서 나오는 에러
>
> `org.apache.oozie.action.hadoop.launcherexception: output data exceeds its limit [2048]	`-> oozie에서 출력이 너무 많아서 생기는 에러. capture output을 끄고 실행하면 됨
>
> oozie에서 폴더 permission denied 됨..-> chmod 변경 

<br/>

##### 18-12-03

- [ ] oozie data_preprocessing
- [x] data_preprocessing encoding 확인
- [ ] get_sentiment 확인
- [x] 실제 데이터로 돌리기

> :two_hearts: -> 이거 왜 안사라지냐 진짜..... U+1F495 다른거 다 사라지는데 왜 혼자... 그만해...제발....
>
> 한글 유니코드로 reg 바꿔서 하니깐 잘됨 ㅎㅎ진작....진짜 중요하다 잊지말아라.. 유니코드...

<br/>

##### 18-12-04

- [x] get_sentiment 확인
- [x] 용언에 -다 붙이기
- [x] spark 결과 -> mysql

> :bug:
>
> `sre_constants.error: bad character range` emoji unicode
>
> python2 버전에서 cmd 한글 출력 안되면 `str(text).decode('utf-8s')`
>
> 용언일 경우 -다 붙이기
>
> mysql 한글 물음표 =>  `$ cd /etc/my.cnf` 추가
>
> ```
> [client]
> default-character-set=utf8
> 
> [mysql]
> default-character-set=utf8
> 
> 
> [mysqld]
> collation-server = utf8_unicode_ci
> init-connect='SET NAMES utf8'
> character-set-server = utf8
> ```
>
> ```sql
> ALTER DATABASE [DB명] DEFAULT CHARACTER SET utf8;
> show variables like ‘c%’
> ```
>
> `spark-submit --packages mysql:mysql-connector-java:5.1.39 [파일 이름]`

<br/>

##### 18-12-05

>:bug:
>
>mysql 위처럼 해도 안됨 -> 보낼 때 url을 `jdbc:mysql://localhost/[db_name]?useUnicode=true&characterEncoding=utf-8` -> 해결
>
>`java.lang.ClassNotFoundException: om.mysql.jdbc.Driver` -> 제플린에 
>
>그래프..pos 나타낼 수 있는게 없음..
>
>pyspark 에서 python 2버전

<br/>

##### 18-12-06

- [x] zeppelin helium 설치
- [x] centos 시간 동기화
- [ ] percent 구하기

> bubble... 제플린..
>
> 즉시 동기화 명령어 `$ chronyc -a makestep`
>
> positive, negative, neutral, complex
>
> :bug:
>
> `RROR: Exception DBusException: org.freedesktop.DBus.Error.AccessDenied` ->
>
> `$ systemctl restart dbus` `$ systemctl restart firewalld`

<br/>

##### 18-12-07

- [x] scraper 2개 파일 얻도록
- [x] multiprocessing
- [x] 전처리 2개
- [x] import hbase
- [x] zeppelin mysql 연동

> :memo:
>
> hbase 여러 값을 저장하려면 versions 지정해주면 됨! -> starbase에는 없음
>
> `alter "test", NAME => "tweets", VERSIONS => 1000000` -> 올바른 방법 아님.. 
>
> :bug:
>
> `Gcc error: gcc: error trying to exec 'cc1': execvp: No such file or directory` -> `$ sudo yum install gcc-c++`
>
> pandas read_json`ValueError: Expected object or value` -> 상대 경로가 잘못된 것
>
> hbase안되었던 것은 create(column family)

<br/>

##### 18-12-08~09

- [x] hbase 연동
- [x] hbase data insert
- [x] sentiment_analysis 정리
- [ ] zeppelin hbase 연동
- [ ] zeppelin 그래프 만들기(날짜)
- [ ] '북한' 데이터 전처리 -> 
- [ ] oozie

> :bug:
>
> ntp 동기화 `systemctl enable ntpd` 이거 하니깐 제대로 작동
>
> 한국시간 `timedatectl set-timezone Asia/Seoul`
>
> `Submitting job to Oozie failed. Please check your definition/configuration. org.apache.oozie.ambari.view.exception.WfmException:`  -> sanbox domain name에 대한 host file 변경

##### 18-12-10

> :bug:
>
> `sudo must be owned by uid 0 and have the setuid bit set` -> `chown root:root /usr/bin/sudo && chmod 4755 /usr/bin/sudo`

> 데이터 시각화
>
> https://zzsza.github.io/development/2018/08/24/data-visualization-in-python/#
>
> https://www.dremio.com/trump-twitter-sentiment-analysis/ > 트럼프

> http://www.zinicap.kr/archives/2433 나중에 이런식으로 수집한 수 아이디 알려주면 될 듯
>
> https://github.com/Ahneunjeong/bigdata-foodelivery/blob/master/배달분석발표자료.pdf
>
> http://wiki.gurubee.net/pages/viewpage.action?pageId=28117507