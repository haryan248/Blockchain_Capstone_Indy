# Blockchain_Capstone
## UNIV. Pass
![KakaoTalk_20210323_135258322](https://user-images.githubusercontent.com/65976572/112437406-43f40180-8d8a-11eb-945d-d7eaaab5edfe.png)
### Kyonggi Univ. Capstone Blockchain Team

경기대학교 컴퓨터공학부
컴퓨터공학캡스톤디자인 수123

팀원: 김성빈, 박기우, 박영길, 조규선, 하현준


## Blockchain and DID and Service

블록체인과 DID를 활용한 분산신원인증 플랫폼과 해당 플랫폼을 활용한 각종 서비스


# Back-End

---

## Docker에서 환경 세팅하기

1. Repository 에서 이미지 pull 받기

    ```bash
    docker pull haryan96/indy:latest
    ```

2. docker exec -itu 0 "컨테이너 이름" /bin/sh

    ```bash
    ~ docker exec -itu 0 capstone /bin/sh
    #
    ```

3. indy-cli 실행

    ```bash
    root@a695d4dc9ab0:/# indy-cli
    indy> exit
    Goodbye...
    root@a695d4dc9ab0:/#
    ```

4. txn 생성
    1. /tmp/indy-sdk/cli 로 이동한다
    2. 임의의 파일을 생성한다. (본 예제에서는 genesis-pool-config로 생성)
        - genesis-pool-config

            {"reqSignature":{},"txn":{"data":{"data":{"alias":"Node1","blskey":"4N8aUNHSgjQVgkpm8nhNEfDf6txHznoYREg9kirmJrkivgL4oSEimFF6nsQ6M41QvhM2Z33nves5vfSn9n1UwNFJBYtWVnHYMATn76vLuL3zU88KyeAYcHfsih3He6UHcXDxcaecHVz6jhCYz1P2UZn2bDVruL5wXpehgBfBaLKm3Ba","blskey_pop":"RahHYiCvoNCtPTrVtP7nMC5eTYrsUA8WjXbdhNc8debh1agE9bGiJxWBXYNFbnJXoXhWFMvyqhqhRoq737YQemH5ik9oL7R4NTTCz2LEZhkgLJzB3QRQqJyBNyv7acbdHrAT8nQ9UkLbaVL9NBpnWXBTw4LEMePaSHEw66RzPNdAX1","client_ip":"127.0.0.1","client_port":9702,"node_ip":"127.0.0.1","node_port":9701,"services":["VALIDATOR"]},"dest":"Gw6pDLhcBcoQesN72qfotTgFa7cbuqZpkX3Xo6pLhPhv"},"metadata":{"from":"Th7MpTaRZVRYnPiabds81Y"},"type":"0"},"txnMetadata":{"seqNo":1,"txnId":"fea82e10e894419fe2bea7d96296a6d46f50f93f9eeda954ec461b2ed2950b62"},"ver":"1"}
            {"reqSignature":{},"txn":{"data":{"data":{"alias":"Node2","blskey":"37rAPpXVoxzKhz7d9gkUe52XuXryuLXoM6P6LbWDB7LSbG62Lsb33sfG7zqS8TK1MXwuCHj1FKNzVpsnafmqLG1vXN88rt38mNFs9TENzm4QHdBzsvCuoBnPH7rpYYDo9DZNJePaDvRvqJKByCabubJz3XXKbEeshzpz4Ma5QYpJqjk","blskey_pop":"Qr658mWZ2YC8JXGXwMDQTzuZCWF7NK9EwxphGmcBvCh6ybUuLxbG65nsX4JvD4SPNtkJ2w9ug1yLTj6fgmuDg41TgECXjLCij3RMsV8CwewBVgVN67wsA45DFWvqvLtu4rjNnE9JbdFTc1Z4WCPA3Xan44K1HoHAq9EVeaRYs8zoF5","client_ip":"127.0.0.1","client_port":9704,"node_ip":"127.0.0.1","node_port":9703,"services":["VALIDATOR"]},"dest":"8ECVSk179mjsjKRLWiQtssMLgp6EPhWXtaYyStWPSGAb"},"metadata":{"from":"EbP4aYNeTHL6q385GuVpRV"},"type":"0"},"txnMetadata":{"seqNo":2,"txnId":"1ac8aece2a18ced660fef8694b61aac3af08ba875ce3026a160acbc3a3af35fc"},"ver":"1"}

    ```bash
    root@a695d4dc9ab0:/# cd tmp/indy-sdk/cli/
    root@a695d4dc9ab0:/tmp/indy-sdk/cli# vi genesis-pool-config
    ```

5. pool 생성과 연결
    1. indy-cli 실행한다
    2. pool create "pool 이름" gen_txn_file=path/to/txn/"txn 이름" 으로 pool을 생성한다
    3. pool connect "pool 이름"으로 pool에 연결할 수 있다

    ```bash
    root@a695d4dc9ab0:/# indy-cli
    indy> pool create 2877 gen_txn_file=/tmp/indy-sdk/cli/genesis-pool-config
    Pool config "2877" has been created
    indy> pool connect 2877
    Pool "2877" has been connected
    pool(2877):indy>
    ```

    - pool 목록 조회하기

        ```bash
        pool(2877):indy> pool list
        +------------+
        | 2877       |
        +------------+
        Current pool "2877"
        ```

    - pool 삭제하기

        ```bash
        indy> pool delete 2877
        Pool "2877" has been deleted.
        ```

6. Wallet 생성과 연결
    1. wallet create "wallet 이름" key 를 통해 지갑을 생성한다
    2. wallet open "wallet 이름" key 를 통해 지갑을 연결한다

    ```bash
    pool(2877):indy> wallet create mywallet key
    Enter value for key:

    Wallet "mywallet" has been created

    pool(2877):indy> wallet open mywallet key
    Enter value for key:

    Wallet "mywallet" has been opened
    pool(2877):mywallet:indy>
    ```

    - 지갑 목록 조회하기

        ```bash
        pool(2877):mywallet:indy> wallet list
        +----------+---------+
        | Name     | Type    |
        +----------+---------+
        | mywallet | default |
        +----------+---------+
        Current wallet "mywallet"
        ```

    - 지갑 삭제하기

        ```bash
        pool(2877):indy> wallet delete mywallet key
        Enter value for key:

        Wallet "mywallet" has been deleted
        ```

7. DID 생성과 사용하기
    1. tmp/indy-sdk 에 admin.txt 파일을 생성한다.
        - admin.txt

            {
            “version”: 1,
            “dids”: [{
            “did”: “Th7MpTaRZVRYnPiabds81Y”,
            “seed”: “000000000000000000000000Steward1”
            }]
            }

    2. did import /path/to/admindid.txt 를 통해 did를 설정한다.
    3. did use "did" 를 통해 did를 사용한다

    ```bash
    pool(2877):mywallet:indy> did import /tmp/indy-sdk/admindid.txt
    Did "Th7MpTaRZVRYnPiabds81Y" has been created with "~7TYfekw4GUagBnBVCqPjiC" verkey
    DIDs import finished
    pool(2877):mywallet:indy> did use Th7MpTaRZVRYnPiabds81Y
    Did "Th7MpTaRZVRYnPiabds81Y" has been set as active
    pool(2877):mywallet:did(Th7...81Y):indy>
    ```

    - 이후 새로운 Did 생성

        ```bash
        pool(2877):mywallet:did(Th7...81Y):indy did new
        Did "55MHkmdAfu9cbqAZNYQRSQ" has been created with "~Ttjgycw8Fk6ELoVoMoBM9K" verkey
        ```

    - Did 목록 조회

        ```bash
        pool(2877):mywallet:did(Th7...81Y):indy> did list
        +------------------------+-------------------------+----------+
        | Did                    | Verkey                  | Metadata |
        +------------------------+-------------------------+----------+
        | Th7MpTaRZVRYnPiabds81Y | ~7TYfekw4GUagBnBVCqPjiC | -        |
        +------------------------+-------------------------+----------+
        | 55MHkmdAfu9cbqAZNYQRSQ | ~Ttjgycw8Fk6ELoVoMoBM9K | -        |
        +------------------------+-------------------------+----------+
        Current did "Th7MpTaRZVRYnPiabds81Y"
        ```

---

## 참조

[Setup Indy SDK build environment for Ubuntu based distro (Ubuntu 16.04) - Hyperledger Indy SDK documentation](https://hyperledger-indy.readthedocs.io/projects/sdk/en/latest/docs/build-guides/ubuntu-build.html)

[hyperledger/indy-sdk](https://github.com/hyperledger/indy-sdk)

[Manage Hyperledger Indy Wallet and DID through Indy CLI and Docker - IBM Developer Recipes](https://developer.ibm.com/recipes/tutorials/manage-hyperledger-indy-wallet-and-did-through-indy-cli-and-docker/)

---

# Python-Wrapper - Pytest

## Indy-Dev-Setup

1. ubuntu 18.04 필요 —> native, vmware 등 가능
2. 터미널
    1. sudo passwd root 설정
    2. git clone [https://github.com/hyperledger/indy-node.git](https://github.com/hyperledger/indy-node.git)
    3. cd indy-node/dev-setup/ubuntu
    4. sh setup-dev-python.sh
    5. sudo -
    6. source ~/.bashrc
    7. sh setup-dev-depend-ubuntu16.sh
3. 깃허브에서 Indy-Node, Indy-Plenum 본인 깃허브로 포크
4. cp [init-dev-project.sh](http://init-dev-project.sh) "본인프로젝트경로"
5. 본인 프로젝트 경로로 이동
6. sh [init-dev-project.sh](http://init-dev-project.sh) "본인 깃허브아이디" "venv 이름" —> 오류 발생(정상)
7. indy-node와 indy-plenum 폴더안에 있는 [setup.py](http://setup.py) 파일에서 'python3-indy==1.15.0-dev-1618'를 찾아 'python3-indy==1.15.0' 으로 변경

```markdown
source /usr/local/bin/virtualenvwrapper.sh
mkvirtualenv -p python3.6 "가상환경이름"
workon "가상환경이름"
pushd indy-node
pip install -e .[tests] # Ubuntu 16 버전에서 오류 발생 (gnu-gcc 오류)
popd
pushd indy-plenum
pip uninstall -y indy-plenum
pip install -e .[tests]
popd
pip install flake8
```

1. workon "venv 이름"
2. 가상환경상에서 파이썬 모듈 다운로드 완료
3. 파이참 설치 후 인터프리터 설정 —> **미해결**

---

## VMware

1. 구글 공유드라이브에 올라간 vmware파일 다운로드
2. vmware로 해당 파일 실행
3. Ubuntu 18.04
4. User: caps
Passwd: kppjh2877
5. /home/caps/[indy-plenum, indy-node] 에 존재
6. 파이참 실행 가능
7. 모듈 로드 오류 발생 —> 미해결

## !! 중요 !!

파이썬 버전을 python -V와 python3 -V
/code

- update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1
- update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2
- update-alternatives --config python3 (3.6버전으로 변경)
- python3 -V 확인 (3.6 버전이어야 함)
- curl [https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py) -o [get-pip.py](http://get-pip.py/)
- python3 [get-pip.py](http://get-pip.py/) —force-reinstall

1. pip install pytest
2. cd /tmp/indy-sdk/wrappers/python/tests
3. pip install —upgrade pip 2번 실행
4. pip install anyio
pip install pytest-asyncio
pip install pytest-tornasync
pip install pytest-trio
pip install pytest-twisted
pip install base58
5. pytest .
    - JSON 형식에서 오류가 발생 —> 미해결

    ![Back-End%20742155db64ec4d9aa97d5d083592dd38/Untitled.png](Back-End%20742155db64ec4d9aa97d5d083592dd38/Untitled.png)

# 가상환경 우분투18.04에서 docker 설치하기

[우분투 18.04 도커(Docker) 설치 방법 - 코스모스팜 블로그](https://blog.cosmosfarm.com/archives/248/%EC%9A%B0%EB%B6%84%ED%88%AC-18-04-%EB%8F%84%EC%BB%A4-docker-%EC%84%A4%EC%B9%98-%EB%B0%A9%EB%B2%95/)

도커 설치 중 이런 오류가 나온다면 아래 아래 사이트에서 해결하기

![Back-End%20742155db64ec4d9aa97d5d083592dd38/Untitled%201.png](Back-End%20742155db64ec4d9aa97d5d083592dd38/Untitled%201.png)

[리눅스 에러 Could not get lock /var/lib/dpkg/lock-frontend](https://kgu0724.tistory.com/71)

# uid 0 is not unique 오류 해결하기

pool_start.sh와 같은 폴더 상에 있는 core.ubuntu.dockerfile의 내용을 다 지우고 아래 내용으로 덮어씌운다

```markdown
FROM indybase

ARG uid=1000

# Install environment
RUN apt-get update -y && apt-get install -y \
        git \
        wget \
        python3.5 \
        python3-pip \
        python-setuptools \
        python3-nacl \
        apt-transport-https \
        ca-certificates
RUN pip3 install -U \
        'pip<10.0.0' \
        setuptools
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys CE7709D068DB5E88
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BD33704C
RUN echo "deb http://us.archive.ubuntu.com/ubuntu xenial main universe" >> /etc/apt/sources.list
RUN echo "deb https://repo.sovrin.org/deb xenial master" >> /etc/apt/sources.list
RUN echo "deb https://repo.sovrin.org/sdk/deb xenial master" >> /etc/apt/sources.list
RUN apt-get update -y
RUN useradd -ms /bin/bash -o -r -u $uid indy
RUN apt-get install -y ursa indy-node libindy
RUN pip3 install python3-indy
USER indy
WORKDIR /home/indy
```

## Indy Pool 구성

- indy-node 구성

```markdown
~$ >> generate_indy_pool_transactions --nodes 4 --clients 5 --nodeNum 1 [--ips '191.177.76.26,22.185.194.102,247.81.153.79,93.125.199.45'] [--network=sandbox]

# --node: 구성하는 노드의 개수
# --client: 접속할 수 있는 클라이언트 개수 (?)
# --nodeNum: 현재 명령어를 실행하는 노드의 노드번호 (뒤의 --ips 순서)
# --ips: 풀을 구성하는 노드의 ip
# --network: 네트워크 이름

~$ >> start_indy_node Node1 "자신의 ip주소" 9701 "자신의 ip주소" 9702
# 9701, 9702: 노드의 순서에 따른 포트 부여 --> 3번째 노드: 9705, 9706

~$ >> cd /var/lib/indy/[풀 이름]
~$ >> ls
# 위에서 설정한 네트워크 이름의 풀 이름
# ls 실행할 경우 pool_transaction_genesis 파일 존재
# pool_transaction_genesis을 열어보면 위에서 구성한 노드들의 IP와 포트번호로 구성되어있음
~$ >> indy-cli

indy >> pool create "풀이름" gen_txn_file=/var/lib/indy/[풀이름]/pool_transaction_genesis
indy >> pool connect "풀이름"
# 풀 생성에 실패할 경우, 새로운 파일을 생성하고 pool_transaction_genesis 내용 붙여넣기 후 저장
# 새로 만든 파일로 pool create
# node가 연결이 되어있지 않을 경우 pool에 접속이 불가능한 것이 정상
```

- indy-client

```markdown
~$ >> cd [자신의 indy-sdk의 pool_transaction_genesis 파일이 있는 경로]
~$ >> vi pool_transaction_genesis

'''
# indy-node에 있는 pool_transaction_genesis 파일 내용 복사하여 붙여넣기
'''
:wq

~$ >> indy-cli
indy >> pool create "풀이름" gen_txn_file=[자신의 indy-sdk의 pool_transaction_genesis 파일이 있는 경로]/pool_transaction_genesis
indy >> pool connect "풀이름"
# indy-sdk에서 pool에 접근하여 wallet, did, transaction 생성 삭제 조회 등 가능
```

### 참조

[Create a Network and Start Nodes - Hyperledger Indy Node documentation](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/start-nodes.html)

---

## indy-sdk python 스크립트 샘플 코드 원리

![Back-End%20742155db64ec4d9aa97d5d083592dd38/_2021._4._11.__9.19.jpg](Back-End%20742155db64ec4d9aa97d5d083592dd38/_2021._4._11.__9.19.jpg)

![Back-End%20742155db64ec4d9aa97d5d083592dd38/_2021._4._11.__9.24.jpg](Back-End%20742155db64ec4d9aa97d5d083592dd38/_2021._4._11.__9.24.jpg)

1. 초기 시작하면 Steward가 생성된다
2. Trust anchor 후보 DID가 생성된다
3. Steward는 2번에서 생성된 DID를 Trust anchor로 등록한다
4. 3번 과정을 수행하기 위한 tx를 발생 시킨다(내용 중 role : 101이 있는데 아마 101이란 역할이 Trust anchor인 것으로 예상됨)
5. 이때 Client가 DID를 생성하는 상황이 생긴다
6. 2~3번 과정을 통해 Trust anchor가 된 Trust anchor가 5번에서 생성된 Client의 DID를 ledger에 등록 시키기 위해 tx를 발생시킨다 (이때 Node 1~4 까지의 검증자가 존재함)
7. Steward는 자신이 등록시킨 Trust anchor의 verkey와 tx로 부터 받아온 verkey의 일치 여부를 통해 자신이 등록한 Trust anchor의 tx가 맞는지를 검증한다

![Back-End%20742155db64ec4d9aa97d5d083592dd38/_2021._4._11.__9.29.jpg](Back-End%20742155db64ec4d9aa97d5d083592dd38/_2021._4._11.__9.29.jpg)

코드를 보면 표시한 곳에 utils로 부터 pool의 txn 파일을 임포트 해오는 것을 확인할 수 있는데 해당 py를 열어보면

![Back-End%20742155db64ec4d9aa97d5d083592dd38/_2021._4._11.__9.29_2.jpg](Back-End%20742155db64ec4d9aa97d5d083592dd38/_2021._4._11.__9.29_2.jpg)

표시한 것과 같이 [utils.py](http://utils.py) 안에는 node 1~4까지의 정보를 담아서 gen_txn_file을 생성하는 부분을 확인할 수 있다. 특정 pool에 연결 시키기 위해서는 해당 부분을 수정하면 될 것으로 예상됨.

nohup start_indy_node Node1 0.0.0.0 9701 0.0.0.0 9702

---

## 파이썬 스크립트 짜는 법

아래 폴더로 이동

```bash
cd /indy-sdk/docs/how-tos/write-did-and-query-verkey/python
```

utils_test.py 로 파이썬 파일 생성

```bash
from os import environ
from pathlib import Path
from tempfile import gettempdir

PROTOCOL_VERSION = 2

# genesis 파일 경로 설정해주는 부분
def get_pool_genesis_txn_path(pool_name):
		# path = '{genesis 파일까지의 경로}'
    path = '/tmp/indy-sdk/cli/test_pool'
    save_pool_genesis_txn_file(path)
    return path

# genesis 파일 내용 넣는 함수
def pool_genesis_txn_data():
		
		# 바로 붙여넣기 하면 안됨 {}가 두번씩 들어가기에 아래 양식대로 넣어야 함
    return "\n".join([
        '{{"reqSignature":{{}},"txn":{{"data":{{"data":{{"alias":"Node1","blskey":"4N8aUNHSgjQVgkpm8nhNEfDf6txHznoYREg9kirmJrkivgL4oSEimFF6nsQ6M41QvhM2Z33nves5vfSn9n1UwNFJBYtWVnHYMATn76vLuL3zU88KyeAYcHfsih3He6UHcXDxcaecHVz6jhCYz1P2UZn2bDVruL5wXpehgBfBaLKm3Ba","blskey_pop":"RahHYiCvoNCtPTrVtP7nMC5eTYrsUA8WjXbdhNc8debh1agE9bGiJxWBXYNFbnJXoXhWFMvyqhqhRoq737YQemH5ik9oL7R4NTTCz2LEZhkgLJzB3QRQqJyBNyv7acbdHrAT8nQ9UkLbaVL9NBpnWXBTw4LEMePaSHEw66RzPNdAX1","client_ip":"118.67.130.224","client_port":9702,"node_ip":"118.67.130.224","node_port":9701,"services":["VALIDATOR"]}},"dest":"Gw6pDLhcBcoQesN72qfotTgFa7cbuqZpkX3Xo6pLhPhv"}},"metadata":{{"from":"Th7MpTaRZVRYnPiabds81Y"}},"type":"0"}},"txnMetadata":{{"seqNo":1,"txnId":"fea82e10e894419fe2bea7d96296a6d46f50f93f9eeda954ec461b2ed2950b62"}},"ver":"1"}}',
        '{{"reqSignature":{{}},"txn":{{"data":{{"data":{{"alias":"Node2","blskey":"37rAPpXVoxzKhz7d9gkUe52XuXryuLXoM6P6LbWDB7LSbG62Lsb33sfG7zqS8TK1MXwuCHj1FKNzVpsnafmqLG1vXN88rt38mNFs9TENzm4QHdBzsvCuoBnPH7rpYYDo9DZNJePaDvRvqJKByCabubJz3XXKbEeshzpz4Ma5QYpJqjk","blskey_pop":"Qr658mWZ2YC8JXGXwMDQTzuZCWF7NK9EwxphGmcBvCh6ybUuLxbG65nsX4JvD4SPNtkJ2w9ug1yLTj6fgmuDg41TgECXjLCij3RMsV8CwewBVgVN67wsA45DFWvqvLtu4rjNnE9JbdFTc1Z4WCPA3Xan44K1HoHAq9EVeaRYs8zoF5","client_ip":"49.50.166.230","client_port":9704,"node_ip":"49.50.166.230","node_port":9703,"services":["VALIDATOR"]}},"dest":"8ECVSk179mjsjKRLWiQtssMLgp6EPhWXtaYyStWPSGAb"}},"metadata":{{"from":"EbP4aYNeTHL6q385GuVpRV"}},"type":"0"}},"txnMetadata":{{"seqNo":2,"txnId":"1ac8aece2a18ced660fef8694b61aac3af08ba875ce3026a160acbc3a3af35fc"}},"ver":"1"}}'
    ])

def save_pool_genesis_txn_file(path):
    data = pool_genesis_txn_data()
    with open(str(path), "w+") as f:
        f.writelines(data)
```

본격적인 스크립트를 실행할 [test.py](http://test.py) 생성

주의) 코드 중 15번은 연결된 Pool을 삭제하는 과정이므로 실제 구성한 Pool을 사용할 때에는 가급적 주석처리해서 사용하지 말것!!

```bash
import asyncio
import json
import pprint

from indy import pool, ledger, wallet, did
from indy.error import IndyError, ErrorCode

from utils_test import get_pool_genesis_txn_path, PROTOCOL_VERSION

# 연결할 Pool의 이름을 입력
pool_name = '12345'
genesis_file_path = get_pool_genesis_txn_path(pool_name)

# [*] 생성하고자 하는 Wallet의 이름(config)과 key(credentials)를 입력
wallet_config = json.dumps({"id": "test_wallet"})
wallet_credentials = json.dumps({"key": "1234"})

def print_log(value_color="", value_noncolor=""):
    """set the colors for text."""
    HEADER = '\033[92m'
    ENDC = '\033[0m'
    print(HEADER + value_color + ENDC + str(value_noncolor))

async def write_nym_and_query_verkey():
    try:
        await pool.set_protocol_version(PROTOCOL_VERSION)

        # 1. genesis 파일의 경로로부터 Pool을 생성 -> 코드 수정을 통해 이미 생성된 Pool을 불러오는 것으로 변경
        print_log('\n1. Creates a new local pool ledger configuration that is used '
                  'later when connecting to ledger.\n')
        pool_config = json.dumps({'genesis_txn': str(genesis_file_path)})
        print_log('genesis_txn: ', genesis_file_path)
        try:
            await pool.create_pool_ledger_config(config_name=pool_name, config=pool_config)
        except IndyError as ex:
            if ex.error_code == ErrorCode.PoolLedgerConfigAlreadyExistsError:
                pass

        # 2. 미리 생성한 Pool을 연결
        print_log('\n2. Open pool ledger and get handle from libindy\n')
        pool_handle = await pool.open_pool_ledger(config_name=pool_name, config=None)

        # 3. [*]에서 설정한 Wallet을 생성
        print_log('\n3. Creating new secure wallet\n')
        try:
            await wallet.create_wallet(wallet_config, wallet_credentials)
            print_log('Wallet Config: ', wallet_config)
            print_log('Wallet Credentials: ', wallet_credentials)
        except IndyError as ex:
            if ex.error_code == ErrorCode.WalletAlreadyExistsError:
                pass

        # 4. 설정한 Wallet을 오픈
        print_log('\n4. Open wallet and get handle from libindy\n')
        wallet_handle = await wallet.open_wallet(wallet_config, wallet_credentials)

        # 5. seed로부터 Steward 설정
        print_log('\n5. Generating and storing steward DID and verkey\n')
        steward_seed = '000000000000000000000000Steward1'
        did_json = json.dumps({'seed': steward_seed})
        steward_did, steward_verkey = await did.create_and_store_my_did(wallet_handle, did_json)
        print_log('Steward DID: ', steward_did)
        print_log('Steward Verkey: ', steward_verkey)

        # 6. 노션 Readme의 'indy-sdk python 스크립트 샘플 코드 원리' 참고
        print_log('\n6. Generating and storing trust anchor DID and verkey\n')
        trust_anchor_did, trust_anchor_verkey = await did.create_and_store_my_did(wallet_handle, "{}")
        print_log('Trust anchor DID: ', trust_anchor_did)
        print_log('Trust anchor Verkey: ', trust_anchor_verkey)

        # 7. 노션 Readme의 'indy-sdk python 스크립트 샘플 코드 원리' 참고
        print_log('\n7. Building NYM request to add Trust Anchor to the ledger\n')
        nym_transaction_request = await ledger.build_nym_request(submitter_did=steward_did,
                                                                 target_did=trust_anchor_did,
                                                                 ver_key=trust_anchor_verkey,
                                                                 alias=None,
                                                                 role='TRUST_ANCHOR')
        print_log('NYM transaction request: ')
        pprint.pprint(json.loads(nym_transaction_request))

        # 8. 노션 Readme의 'indy-sdk python 스크립트 샘플 코드 원리' 참고
        print_log('\n8. Sending NYM request to the ledger\n')
        nym_transaction_response = await ledger.sign_and_submit_request(pool_handle=pool_handle,
                                                                        wallet_handle=wallet_handle,
                                                                        submitter_did=steward_did,
                                                                        request_json=nym_transaction_request)
        print_log('NYM transaction response: ')
        pprint.pprint(json.loads(nym_transaction_response))

        # 9. 노션 Readme의 'indy-sdk python 스크립트 샘플 코드 원리' 참고
        print_log('\n9. Generating and storing DID and verkey representing a Client '
                  'that wants to obtain Trust Anchor Verkey\n')
        client_did, client_verkey = await did.create_and_store_my_did(wallet_handle, "{}")
        print_log('Client DID: ', client_did)
        print_log('Client Verkey: ', client_verkey)

        # 10. 노션 Readme의 'indy-sdk python 스크립트 샘플 코드 원리' 참고
        print_log('\n10. Building the GET_NYM request to query trust anchor verkey\n')
        get_nym_request = await ledger.build_get_nym_request(submitter_did=client_did,
                                                             target_did=trust_anchor_did)
        print_log('GET_NYM request: ')
        pprint.pprint(json.loads(get_nym_request))

        # 11. 노션 Readme의 'indy-sdk python 스크립트 샘플 코드 원리' 참고
        print_log('\n11. Sending the Get NYM request to the ledger\n')
        get_nym_response_json = await ledger.submit_request(pool_handle=pool_handle,
                                                           request_json=get_nym_request)
        get_nym_response = json.loads(get_nym_response_json)
        print_log('GET_NYM response: ')
        pprint.pprint(get_nym_response)

        # 12. 노션 Readme의 'indy-sdk python 스크립트 샘플 코드 원리' 참고
        print_log('\n12. Comparing Trust Anchor verkey as written by Steward and as retrieved in GET_NYM '
                  'response submitted by Client\n')
        print_log('Written by Steward: ', trust_anchor_verkey)
        verkey_from_ledger = json.loads(get_nym_response['result']['data'])['verkey']
        print_log('Queried from ledger: ', verkey_from_ledger)
        print_log('Matching: ', verkey_from_ledger == trust_anchor_verkey)
        
        # 13. 마무리(1) Wallet과 Pool을 삭제하기 전에 닫아줌
        print_log('\n13. Closing wallet and pool\n')
        await wallet.close_wallet(wallet_handle)
        await pool.close_pool_ledger(pool_handle)

        # 14. 마무리(2) Wallet을 먼저 삭제함
        print_log('\n14. Deleting created wallet\n')
        await wallet.delete_wallet(wallet_config, wallet_credentials)
        '''
        # 15. 마무리(3) Pool을 삭제함
        print_log('\n15. Deleting pool ledger config\n')
        await pool.delete_pool_ledger_config(pool_name)
        '''
    except IndyError as e:
        print('Error occurred: %s' % e)

def main():
    loop = asyncio.get_event_loop()
    loop.run_until_complete(write_nym_and_query_verkey())
    loop.close()

if __name__ == '__main__':
    main()
```

 위 코드에서 1 ~ 12 과정을 진행하면 Wallet에 Stewrd, Trust anchor, Client 총 세 개의 DID가 생성된다. 그러나 사용자가 따로 indy-cli를 실행하면 생성한 Wallet의 이름과 verkey로 'wallet open' 명령어를 넣더라도 오류가 나오며 접속이 되지 않는다. 이러한 이유는 Wallet이 indy-cli에 attach하지 않아서 그렇다.

 아래의 방법을 참고하자.

```bash
pool(12345):indy> wallet list
There are no wallets
pool(12345):indy> wallet open test_wallet key
Enter value for key:

Wallet "test_wallet" isn't attached to CLI
```![_2021 _4 _11 __9 19](https://user-images.githubusercontent.com/65976572/116072253-d0ffe280-a6c9-11eb-8440-420a70353be7.jpg)
![_2021 _4 _11 __9 24](https://user-images.githubusercontent.com/65976572/116072265-d3fad300-a6c9-11eb-9147-4f2e50fdd6d9.jpg)
![_2021 _4 _11 __9 29_2](https://user-images.githubusercontent.com/65976572/116072273-d4936980-a6c9-11eb-910e-0f743c9d43da.jpg)
![_2021 _4 _11 __9 29](https://user-images.githubusercontent.com/65976572/116072278-d52c0000-a6c9-11eb-837a-93f80ff21377.jpg)
![Untitled 1](https://user-images.githubusercontent.com/65976572/116072282-d5c49680-a6c9-11eb-8ba0-104c1957ccf2.png)
![Untitled](https://user-images.githubusercontent.com/65976572/116072284-d5c49680-a6c9-11eb-9011-f56bf7590c0c.png)


```bash
pool(12345):indy> wallet attach test_wallet
Wallet "test_wallet" has been attached
pool(12345):indy> wallet list
+-------------+---------+
| Name        | Type    |
+-------------+---------+
| test_wallet | default |
+-------------+---------+
```

이때 did list를 이용하면 위 코드를 실행하며 생성된 DID들이 들어있는 것을 확인할 수 있다.

```bash
pool(12345):test_wallet:indy> did list
+------------------------+-------------------------+----------+
| Did                    | Verkey                  | Metadata |
+------------------------+-------------------------+----------+
| WrHY37BUucCnFB1K9G9wWG | ~Gu3A8uNeewzwisqhk5y3xH | -        |
+------------------------+-------------------------+----------+
| Th7MpTaRZVRYnPiabds81Y | ~7TYfekw4GUagBnBVCqPjiC | -        |
+------------------------+-------------------------+----------+
| 9mLoiE5hMBEyzgiiaqc9wa | ~J3twGKkXZm7rUWz8NAUV4e | -        |
+------------------------+-------------------------+----------+
```

아래와 같은 방법을 이용하면 이미 생성된 지갑에 새로운 DID만 추가하는 것이 가능하다

```bash
import asyncio
import json
import pprint

from indy import pool, ledger, wallet, did
from indy.error import IndyError, ErrorCode

from utils_test import get_pool_genesis_txn_path, PROTOCOL_VERSION

pool_name = '12345'
genesis_file_path = get_pool_genesis_txn_path(pool_name)

wallet_config = json.dumps({"id": "test_wallet"})
wallet_credentials = json.dumps({"key": "1234"})

def print_log(value_color="", value_noncolor=""):
    """set the colors for text."""
    HEADER = '\033[92m'
    ENDC = '\033[0m'
    print(HEADER + value_color + ENDC + str(value_noncolor))

async def write_nym_and_query_verkey():
    try:
        await pool.set_protocol_version(PROTOCOL_VERSION)
        # Step 2 code goes here.

        # Tell SDK which pool you are going to use. You should have already started
        # this pool using docker compose or similar. Here, we are dumping the config
        # just for demonstration purposes.
        print_log('\n1. Creates a new local pool ledger configuration that is used '
                  'later when connecting to ledger.\n')
        pool_config = json.dumps({'genesis_txn': str(genesis_file_path)})
        print_log('genesis_txn: ', genesis_file_path)
        try:
            await pool.create_pool_ledger_config(config_name=pool_name, config=pool_config)
        except IndyError as ex:
            if ex.error_code == ErrorCode.PoolLedgerConfigAlreadyExistsError:
                pass

        print_log('\n2. Open ledger and get handle\n')
        pool_handle = await pool.open_pool_ledger(config_name=pool_name, config=None)

        print_log('\n4. Open identity wallet and get handle\n')
        wallet_handle = await wallet.open_wallet(wallet_config, wallet_credentials)

        # Step 3 code goes here.

        # First, put a steward DID and its keypair in the wallet. This doesn't write anything to the ledger,
        # but it gives us a key that we can use to sign a ledger transaction that we're going to submit later.
        print_log('\n5. Generate and store steward DID and verkey\n')
        # The DID and public verkey for this steward key are already in the ledger; they were part of the genesis
        # transactions we told the SDK to start with in the previous step. But we have to also put the DID, verkey,
        # and private signing key into our wallet, so we can use the signing key to submit an acceptably signed
        # transaction to the ledger, creating our *next* DID (which is truly new). This is why we use a hard-coded seed
        # when creating this DID--it guarantees that the same DID and key material are created that the genesis txns
        # expect.
        steward_seed = '000000000000000000000000Steward1'
        did_json = json.dumps({'seed': steward_seed})
        steward_did, steward_verkey = await did.create_and_store_my_did(wallet_handle, did_json)
        print_log('Steward DID: ', steward_did)
        print_log('Steward Verkey: ', steward_verkey)

        # Now, create a new DID and verkey for a trust anchor, and store it in our wallet as well. Don't use a seed;
        # this DID and its keyas are secure and random. Again, we're not writing to the ledger yet.
        print_log('\n6. Generating and storing trust anchor DID and verkey\n')
        trust_anchor_did, trust_anchor_verkey = await did.create_and_store_my_did(wallet_handle, "{}")
        print_log('Trust anchor DID: ', trust_anchor_did)
        print_log('Trust anchor Verkey: ', trust_anchor_verkey)

        # Step 4 code goes here.

        # Step 5 code goes here.

    except IndyError as e:
        print('Error occurred: %s' % e)

def main():
    loop = asyncio.get_event_loop()
    loop.run_until_complete(write_nym_and_query_verkey())
    loop.close()

if __name__ == '__main__':
    main()
```

아래와 같이 새로운 Trust anchor의 DID가 기존의 Wallet에 추가되었다.

```bash
pool(12345):test_wallet:indy> did list
+------------------------+-------------------------+----------+
| Did                    | Verkey                  | Metadata |
+------------------------+-------------------------+----------+
| 2yCHsPzYvEAjPFDNvkQwQw | ~Fub6B1Mvvui15UDQxxJrYq | -        |
+------------------------+-------------------------+----------+
| WrHY37BUucCnFB1K9G9wWG | ~Gu3A8uNeewzwisqhk5y3xH | -        |
+------------------------+-------------------------+----------+
| Th7MpTaRZVRYnPiabds81Y | ~7TYfekw4GUagBnBVCqPjiC | -        |
+------------------------+-------------------------+----------+
| 9mLoiE5hMBEyzgiiaqc9wa | ~J3twGKkXZm7rUWz8NAUV4e | -        |
+------------------------+-------------------------+----------+
```

### 참고

[hyperledger/indy-sdk](https://github.com/hyperledger/indy-sdk/tree/master/docs/how-tos/write-did-and-query-verkey)

# api server 접속 및 run

1. putty 106.10.32.81 : 2877   비밀번호 = root
2. cd home/caps/venv0/bin → source activate
3. caps/rest_server → python [manage.py](http://manage.py) makemigrations memeber , python manage.py migrate member
4. python [manage.py](http://manage.py) runserver 0.0.0.0:8000
5. 외부 웹 브라우저에서 [http://101.101.218.36:8000/api/members](http://101.101.218.36:8000/api/members) 접속

    관리자 계정 admin / admin

- api 서버에서 mysql 접속
    - mysql -u root -p  → 패스워드 : root
