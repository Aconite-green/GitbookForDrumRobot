---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Basic Linux Commands

## \[ Navigating the File System ]

### `ls`, `cd`, `pwd` Commands

<table><thead><tr><th width="142">Commands</th><th>Function</th></tr></thead><tbody><tr><td>ls</td><td><strong>현재 디렉토리의 파일 및 폴더 목록을 표시합니다</strong>. -l 옵션을 사용하면 자세한 정보를 볼 수 있고, -a 옵션을 사용하면 숨겨진 파일과 폴더도 표시합니다.</td></tr><tr><td>cd [디렉토리]</td><td><strong>특정 디렉토리로 이동합니다.</strong> 예를 들어, cd Documents 명령은 사용자의 'Documents' 폴더로 이동합니다. cd .. 명령은 상위 디렉토리로 이동합니다.</td></tr><tr><td>pwd</td><td>현재 작업 중인 디렉토리의 <strong>전체 경로를 표시</strong>합니다.</td></tr></tbody></table>

### Directory Structure in Linux

<table><thead><tr><th width="140">Directory</th><th>Explanation</th></tr></thead><tbody><tr><td>/</td><td>루트 디렉토리로, 리눅스 파일 시스템의 <strong>최상위에 위치</strong>합니다.</td></tr><tr><td>/home</td><td><strong>사용자의 개인 데이터와 설정 파일을 저장</strong>하는 디렉토리입니다. 각 사용자에게는 /home/[사용자 이름] 형태의 디렉토리가 할당됩니다.</td></tr><tr><td>/etc</td><td><strong>시스템 설정 파일</strong>을 저장하는 디렉토리입니다.</td></tr><tr><td>/var</td><td><strong>로그 파일</strong>과 같은 변동성이 있는 데이터를 저장하는 디렉토리입니다.</td></tr><tr><td>/usr</td><td><strong>사용자 프로그램과 라이브러리</strong>를 저장하는 디렉토리입니다.</td></tr></tbody></table>

## \[ File Operations ]

### `cat`, `touch`, `rm`, `mv`, `cp Commands`

<table><thead><tr><th width="231">Commands</th><th>Explanation</th></tr></thead><tbody><tr><td>cat [파일명]</td><td><strong>파일의 내용을 화면에 표시</strong>합니다.</td></tr><tr><td>touch [파일명]</td><td><strong>새 파일을 생성</strong>하거나, 기존 파일의 타임스탬프를 업데이트합니다.</td></tr><tr><td>rm [파일명]</td><td><strong>파일을 삭제</strong>합니다. -r 옵션을 사용하면 디렉토리와 그 안의 모든 내용을 삭제할 수 있습니다.</td></tr><tr><td>mv [원본 파일] [대상 위치]</td><td><strong>파일을 이동하거나 이름을 변경</strong>합니다.</td></tr><tr><td>cp [원본 파일] [대상 위치]</td><td><strong>파일을 복사</strong>합니다.</td></tr></tbody></table>

## \[ System Monitoring and Management ]

### `top`, `ps`, `free`, `df`, `du Commands`

<table><thead><tr><th width="146">Commands</th><th>Explanation</th></tr></thead><tbody><tr><td>top</td><td>현재 시스템에서 실행 중인 <strong>프로세스와 관련된 정보</strong>를 실시간으로 표시합니다. CPU와 메모리 사용량을 확인할 수 있습니다.</td></tr><tr><td>ps</td><td>현재 실행 중인 <strong>프로세스 목록</strong>을 표시합니다. -aux 옵션을 사용하면 모든 프로세스에 대한 자세한 정보를 볼 수 있습니다.</td></tr><tr><td>free</td><td><strong>시스템의 메모리 사용량</strong>을 표시합니다. -m 옵션을 사용하면 메가바이트 단위로 정보를 볼 수 있습니다.</td></tr><tr><td>df</td><td>파일 시스템의 <strong>디스크 사용량</strong>을 표시합니다. -h 옵션을 사용하면 용량을 사람이 읽기 쉬운 형태로 표시합니다.</td></tr><tr><td>du</td><td><strong>특정 디렉토리나 파일의 디스크 사용량</strong>을 표시합니다. -h 옵션을 사용하면 용량을 사람이 읽기 쉬운 형태로 표시합니다.</td></tr></tbody></table>

## \[ Managing Processes and System Resources ]

### 1. Kill 명령어

* `kill` 명령어는 실행 중인 프로세스를 종료시키는 데 사용됩니다.
* 예시: 프로세스 ID가 1234인 프로세스를 종료하려면, 다음과 같이 입력합니다.

<pre class="language-bash"><code class="lang-bash"><strong>kill 1234
</strong></code></pre>

* 프로세스가 종료되지 않을 경우, `-9` 옵션을 사용하여 강제로 종료할 수 있습니다.

```bash
kill -9 1234
```

{% hint style="info" %}
명령어 사용 시 주의해야 할 점은, 특히 **`kill -9`** 같은 강제 종료 옵션은 프로세스가 중요한 작업을 처리 중일 때 사용하면 **데이터 손실이나 다른 문제**를 일으킬 수 있으므로 신중하게 사용해야 합니다.
{% endhint %}

### 2. Nice 명령어

* `nice` 명령어는 프로세스를 시작할 때 우선순위(niceness)를 설정합니다. 이 값은 -20(가장 높은 우선순위)부터 19(가장 낮은 우선순위)까지입니다.
* 예시: 새로운 프로세스를 nice 값 10으로 시작하려면, 다음과 같이 입력합니다.

```bash
nice -n 10 command
```

* 여기서 `command`는 시작하려는 프로세스나 프로그램입니다.

### 3. Renice 명령어

* `renice` 명령어는 이미 실행 중인 프로세스의 우선순위를 변경합니다.
*   예시: 프로세스 ID가 1234인 프로세스의 우선순위를 5로 변경하려면, 다음과 같이 입력합니다.

    ```bash
    renice 5 1234
    ```

### 4. Cron 과 Crontab 명령어

* `cron`은 리눅스에서 시간 기반으로 작업을 스케줄링하는 데몬입니다. `crontab` 파일은 이러한 스케줄링 작업을 정의합니다.
* 예시: 매일 밤 11시에 특정 스크립트를 실행하려면, `crontab` 파일에 다음과 같이 입력합니다.

```bash
0 23 * * * /path/to/script.sh
```

* 여기서 `0 23 * * *`는 매일 밤 11시를 의미하고, `/path/to/script.sh`는 실행할 스크립트의 경로입니다.
