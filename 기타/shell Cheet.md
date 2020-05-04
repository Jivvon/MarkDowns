### background mode로 켜놓고 안 보이게 하려면

```shell
$ SIGSTP (ctrl + z)
$ bg
$ disown -h [jobspec]
```

SIGSTP : 프로세스 일시정지 (중단 아님)<br />
bg : background mode<br />
disown -h : -h 옵션을 사용하면 *jobspec*은 job list에서 삭제되지 않지만 쉘이 SIGHUP을 수신하여도 SIGHUP을 해당 작업에 보내지 않는다.

> `bg`는 작업을 백그라운드로 보내는 명령어이다. (명령어 마지막에 `&`를 붙여주면 같은 기능을 한다.) 이는 stdin을 block하고 job이 끝날 때까지 shell이 기다리지 않게 해준다.

> `disown`은 shell의 job list에서 job을 제거하여 세션을 종료했을 때 SIGHUP로부터 보호받을 수 있다. (-h 옵션을 통해 제거하지 않을 수 있다.) but, 여전히 터미널에 연결되어 있으므로 터미널이 destroyed되면 프로그램도 stdin에서 내용을 읽어오려하거나 stdout에 무언가를 쓰려는 순간 꺼진다.
> 
> 쉽게 말해서 disown은 프로세스에 대한 소유권을 포기하는 것이다. 나의 job 리스트에 있는 것이 아니므로 **내가 접속을 끊어도 작업은 background에서 계속 돌아간다**.

```shell
$ netstat -nap | grep 8888
```

위 명령어로 확인할 수 있다.

-n : number port<br />-a : all<br />-p : PID



### 프로세스

##### ssh 이름이 들어가는 프로세스 정보 표시

```shell
$ ps -ef | grep ssh
```

-e : 모든 프로세스 표시
-f : 프로세스의 정보를 더 많이 출력

##### 프로세스 중지(kill)

```shell
$ kill -9 `ps -ef | grep 'PROCESS_NAME' | awk '{print $2}'`
$ kill -KILL `lsof -t -u jiwon`
```

kill -9 : PID로 프로세스를 중지
awk '{print $2}' : 검색된 줄에서 2번째 항목(PID) 출력<br />-KILL `lsof -t -u jiwon` : jiwon 사용자의 모든 프로세스 중지



#### lsof (list of files)

```shell
lsof
lsof -u jiwon
lsof /var/log/httpd/access_log
lsof -i TCP:22
lsof -i 4
lsof -t -u jiwon
```
lsof : 모든 열린 파일 출력<br />-u jiwon : 특정 사용자(jiwon)의 열린 파일 출력<br />lsof /var/log/httpd/access_log : 특정 파일을 사용하는 프로세스 보기<br />-i TCP:22 : TCP 프로토콜의 22번 포트를 사용하는 프로세스 정보를 출력. (22-80처럼 범위 지정도 가능)<br />-i 4 : IPv4 버전만 확인. (IPv6일 경우 6)<br />-t : 자세한 정보를 출력하지 않고 pid만 출력.<br />-u jiwon : jiwon 사용자로 구동한 프로세스



### ssh

[여기](https://jivvon.github.io/2020/04/29/ssh-command.html)



### vim

[여기](https://jivvon.github.io/2020/02/24/vim-cheet.html)



##### 우분투 지금 종료

```shell
sudo shutdown -h now
```



###### 더 알아보자

```shell
$ find ./Girlgroup ! -name "Sistar" -exec grep -r "Sexy" {} \;
$ find ./Girlgroup ! -name "Sistar" | xargs grep "Sexy"
```
위의 내용은 [여기](https://kldp.org/node/155637)

```shell
$ find . -exec mycommand '{}' '+'
```






###### 참고

- [몇 년 전부터 알았으면 하는 리눅스 명령어는?](https://www.reddit.com/r/linux/comments/mi80x/give_me_that_one_command_you_wish_you_knew_years/)  1등이 bash의 내장코맨드인 `disown` 이군요. 쉘종료하면 죽어버리는 Job들에 대해 내꺼아냐! 라고 선언해서 안죽이게 하는 명령
- [제타위키 리눅스](https://zetawiki.com/wiki/리눅스)
- [구루의 기술뉴스](https://xguru.net) 