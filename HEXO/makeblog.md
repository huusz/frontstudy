# Hexo와 github page로 블로그 만들기 
###### 2017.07.22 / 1st Weekend
## About GIT

##### Git 로컬 저장소 만들기

_Git bash command_

```cd 디렉터리```를 이용하여 **원하는 디렉터리로 이동**한다. 만약 디렉터리 내에서 새로운 폴더를 생성하고 싶다면 ```mkdir 폴더명```을 작성하면 된다.  

```git init```: 해당 폴더를 초기화한다. 폴더 내부에 파일이 존재해도, 아무것도 없어도 상관 없다. 해당 폴더는 초기화되면서 git 로컬 저장소로 설정된다. 아래 메시지는 성공적으로 초기화 된 경우에 나타난다.
```
Initialized empty Git repository in 디렉터리/.git/
```
그리고 터미널 상에서 디렉터리 끝 부분에 ```(master)```가 추가된 것을 확인할 수 있을 것이다.  
git 로컬 저장소로 설정된 폴더 내부에는 숨김 파일 형식의 .git 폴더가 만들어진다. .git 폴더에는 로컬저장소로 설정한 폴더의 모든 git관리 정보들(commit, push ..)이 쌓인다.

---


## Makes blog

### Make a hexo
[Hexo](https://hexo.io/docs/)는 Node.js기반 정적 사이트 생성기이다. 마크다운으로 글을 쓰고, 게시할 수 있으며 테마를 적용한 정적(static) 파일을 생성한다.

#### Install
Hexo를 설치하려면 Node.js와 Git이 설치되어 있어야 한다.

* [Node.js](http://nodejs.org/)

* [Git](http://git-scm.com/)

이미 설치되어 있다면 이 과정은 넘어가고, 바로 npm을 통해 Hexo를 설치할 수 있다.

> Mac OS 사용자라면, 먼저 앱스토어에서 Xcode를 설치한다. 설치가 완료되면 Xcode를 열고 Preferences-> Download -> Command Line Tools -> Install을 순서대로 진행한다.

git bash에서 아래 명령어로 Hexo를 설치할 수 있다.
```
$ npm install -g hexo-cli
```

#### Setup
Hexo 설치가 완료되면, 아래 명령을 순서대로 진행하여 폴더를 초기화한다. '폴더디렉터리'는 hexo로 만든 블로그를 관리할 폴더의 디렉터리(경로)이다.
```
$ hexo init 폴더디렉터리
$ cd 폴더디렉터리
$ npm install
```
초기화가 완료되면, 해당 폴더가 '프로젝트 폴더'가 된다. 폴더 내부 구조는 아래와 같을 것이다.
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
설치가 완료되면 아래 명령어를 통해 로컬 서버를 통해 hexo로 만들어진 블로그를 확인할 수 있다.
```
$ hexo server
```
[http://localhost:4000](http://localhost:4000)

### Connect Github page

1) github에서 username(내 계정의 유저네임).githun.io라는 이름의 repository를 public으로 만든다.

2) git bash에서 ```cd 디렉터리``` 명령어를 이용해 블로그 관리용 폴더로 이동한다. ```mkdir 폴더명```으로 새 폴더를 만들어도 된다. 새폴더를 만든 경우엔 ```cd 폴더명```으로 해당 폴더로 이동해주는 것도 잊지 말것!

3) ```git init``` 명령어를 입력하여 해당 폴더를 git 관리 폴더로 지정한다. 이제 이 폴더가 내 PC의 로컬 저장소이다.

4) 다음으로, 로컬저장소와 github에서 만든 원격저장소를 연결해 줄 차례이다. git bash에서 ```git remote add origin 레파지토리주소(HTTPS)```를 입력한다. 레파지토리 주소는 .git으로 끝나도록 한다.  
EX) ``` git remote add origin https://github.com/huusz/huusz.github.io.git ```

5) 원격 저장소는 아직 비어있는 상태이므로 로컬 저장소와 같게 만들어 줄 것이다. ```git status``` 명령어로 현재 git의 상태가 어떠한지 확인한다. 로컬 저장소에는 앞서 만든 hexo 파일들이 있다. 따라서 변경 사항이 있다는 메시지가 발생할 것이다. 

6) 변경 사항 ('hexo 관련 파일들이 추가되었음')을 원격 저장소에 반영하기 위해서는 먼저 stage에 올려야 한다. ```git add --all ``` 을 하면 변경된 모든 사항을 stage에 올릴 수 있다. stage에 올린다는 것은 반영(commit)할 준비를 한다는 것을 의미한다. 만약 선택적으로 stage에 올리고 싶다면 ```git all 폴더명 또는 파일명``` 명령을 사용할 수 있다.

7) 이제 stage에 올린 변경 사항들을 반영해줄 차례다. 이를 commit 이라고 한다. ```git commit -m "커밋메시지"``` 명령을 사용하면 모든 변경 사항을 적용할 수 있다.

8) ```git push -u origin master``` 명령으로 최종적으로 원격 저장소에 변경 사항을 적용한다.

### Deploy

1) ```_config.yml```에 GitHub 정보를 입력한다.
```
deploy:
  type: git
  repo: https://github.com/huusz/huusz.github.io.git
  (또는 SSH 형태로)
  branch: master
```

2) 정적 파일을 생성한다.
```
$ hexo generate
```
만약 제대로 파일이 생성되지 않으면, ```hexo clean```을 통해 ```public``` 디렉토리를 비운 후 ```hexo generate```를 사용하면 된다.

3) deploy를 위한 플러그인을 설치한다.
```
$ npm install --save hexo-deployer-git
```
```.deploy_git```폴더가 디렉토리 내에 잘 생성되었다면,

4) deploy 명령어를 사용한다.
```
$ hexo deploy
```

5) deploy가 완료되면 username.github.io를 URL로 하는 접속 가능한 블로그가 만들어진 것을 브라우저를 통해 확인할 수 있다.