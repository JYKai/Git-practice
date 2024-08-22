# 깃과 브랜치

## 브랜치란?

### 브랜치 기능 살펴보기
브랜치는 커밋을 가리키는 포인터와 비슷하다고 생각하면 된다.
- 기존에 저장한 파일을 main 브랜치에 그대로 유지하면서 기존 파일 내용을 수정하거나 새로운 기능을 구현할 파일을 만들 수 있는데, main 브랜치에서 뻗어 나오는 새 브렌치를 만드는 것을 **'분기(branch)한다'**고 한다.
- 새 브랜치에서 작업을 완료한 뒤 원래 main 브랜치에 합하는 것을 **'병합(merge)한다'**고 한다.

</br>

## 브랜치 만들기

### 실습 상황 설정하기
1. 터미널 창을 열어 홈 디렉터리에 manual이라는 새 디렉터리를 만들고 이동한다.
```
$ mkdir manual
$ cd manual
```

2. manual 디렉터리를 깃 저장소로 만든다.
```
$ git init
$ ls -al
```

3. work.txt 파일을 생성한 뒤, 'content 1'이라는 내용을 입력한다.
```
$ vim work.txt
```

4. 만든 파일을 스테이지에 올리고 커밋한다.
```
$ git add work.txt
$ git commit -m "work 1"
```

5. work.txt 파일을 내용을 수정해서 두 번 더 커밋한다.

6. 커밋을 완료한 뒤 ```git log```를 통해 커밋 내역을 확인한다.
- work 1, work 2, work 3 커밋들을 확인할 수 있다.
- main 브랜치가 가장 최신 커밋인 'work 3'를 가리키고 있고, HEAD가 main 브랜치를 가리키고 있다.
    - HEAD는 여러 브랜치 중에서 현재 작업 중인 브랜치를 가리킨다.

### 새 브랜치 만들기
apple, google, ms라는 고객사가 있다고 가정하고 각 고객사에 맞는 작업을 진행한다.

1. ```git branch```를 통해 브랜치를 만들거나 확인할 수 있다.

2. 새로운 브랜치를 만든다. 
```
$ git branch apple
```
- main 브랜치 위에 apple 브랜치가 추가된 것을 확인할 수 있다.
- main 앞에 * 표시는 아직 우리가 main 브랜치에서 작업하고 있다는 뜻이다.
- 브랜치가 추가된 후에는 커밋 로그 화면도 다르게 나타난다.
    - (HEAD -> main) -> (HEAD -> main, apple)
    - 저장소에 두 개의 브랜치가 있고, HEAD -> main 이므로 현재 작업 중인 브랜치는 main 브랜치라는 뜻이다.

3. google, ms 브랜치를 추가한다.


### 브랜치 사이 이동하기 - git checkout

1. main 브랜치에서 새로운 커밋을 만들어 어떻게 달라지는지 확인해본다.
```
$ vim work.txt <= work.txt에 'main content 4' 추가 입력 후 저장
$ git commit -am "main content 4"
```

2. ```git log --oneline```을 통해 로그를 확인한다.
- ```--oneline``` 옵션은 한 줄에 한 커밋씩 나타내 주기 때문에 커밋을 간략히 확인할 때 편리하다.
    ```
    f897b5a (HEAD -> main) main content 4
    b66be1c (ms, google, apple) work 3
    25428a5 work 2
    734ddef work 1
    ```

3. ```git checkout 브랜치명``` 명령을 통해 다른 브랜치로 이동(체크아웃)한다.
```
$ git checkout apple
```
- ```git log --oneline```을 통해 로그를 확인한다.
    ```
    b66be1c (HEAD -> apple, ms, google) work 3
    25428a5 work 2
    734ddef work 1
    ```
    - apple 브랜치의 최신 커밋은 처음 분기될 때인 'work 3' 커밋 그대로이다. ```cat work.txt```를 통해 확인해본다.
        ```
        content 1
        content 2
        content 3
        ```
    - main 브랜치에서 입력했던 'main content 4'가 존재하지 않는다.

</br>

## 브랜치 정보 확인하기
여러 브랜치에서 각각 커밋이 이루어질 때 커밋끼리 어떤 관계를 하고 있는지 확인하는 방법과 브랜치 사이의 차이점을 확인하는 방법을 알아본다.

### 새 브랜치에 커밋하기
1. apple 브랜치의 work.txt 파일에 'apple content 4'라는 텍스트를 추가하고 저장한다.

2. apple.txt 파일을 새로 생성한 뒤 'apple content 4'라는 텍스트를 입력하고 저장한다.

3. 수정된 파일들을 커밋한다.
```
$ git add .
$ git commit -m "apple content 4"
```

4. 로그를 확인해본다.
```
8d84f4a (HEAD -> apple) apple content 4
b66be1c (ms, google) work 3
25428a5 work 2
734ddef work 1
```

5. ```git log``` 명령을 사용할 때 ```--branches``` 옵션을 사용하면 각 브랜치의 커밋을 함께 볼 수 있다.
```
$ git log --oneline --branches
```
```
8d84f4a (HEAD -> apple) apple content 4
f897b5a (main) main content 4
b66be1c (ms, google) work 3
25428a5 work 2
734ddef work 1
```
- 어떤 브랜치에서 만든 커밋인지 구별할 수 있다.

6. 브랜치와 커밋의 관계를 좀 더 보기 쉽게 그래프 형태로 표시하려면 ```git log``` 명령어에 ```--graph``` 옵션을 함께 사용한다.
```
$ git log --oneline --branches -- graph
```
```
* 8d84f4a (HEAD -> apple) apple content 4
| * f897b5a (main) main content 4
|/  
* b66be1c (ms, google) work 3
* 25428a5 work 2
```
- apple 브랜치는 'work 3' 커밋 다음 만들어졌다는 것을 확인할 수 있다.
- main 브랜치 또한 'work 3' 커밋을 부모 커밋으로 가지고 있다.


### 브랜치 사이의 차이점 알아보기
브랜치마다 커밋이 점점 쌓여갈수록 브랜치 사이에 어떤 차이가 있는지 일일 확힌하기 어려워진다.
- 브랜치 이름 사이에 마침표 두 개(..)를 넣는 명령으로 차이점을 쉽게 확인할 수 있다.
- 마침표 왼쪽에 있는 브랜치를 기준으로 오른쪽 브랜치와 비교한다.
```
$ git log main..apple
```
```
commit 8d84f4ae024d3ee8afdf9822e02c1d689b595d5f (HEAD -> apple)
Author: JYKai <noahyun1222@gmail.com>
Date:   Wed Jun 19 20:50:53 2024 +0900

    apple content 4
```
- main 브랜치에는 없고 apple 브랜치에만 있는 커밋, 즉 'apple content 4' 커밋을 볼 수 있다.

</br>

## 브랜치 병합하기
브랜치와 브랜치를 병합하다 보면 여러 상황이 생길 수 있는데 각 상황마다 병합하는 방법을 알아본다. 또한, 충돌이 있을 때 해결하는 방법도 함께 살펴본다.

### 서로 다른 파일 병합하기
1. 새로운 디렉터리를 만들어 환경을 설정한다.
- ```git init 디렉터리_이름``` : 새로운 디렉터리를 생성하고 저장소를 초기화하는 과정을 한 번에 처리할 수 있다.

2. 만들어진 디렉터리에서 work.txt 파일을 생성한 뒤, '1'이라는 내용을 입력한 후 저장한다. 이후, 'work 1'이라는 메세지로 커밋한다.

3. 새로운 브랜치 'o2'를 만든다.

4. 현재 main 브랜치에 main.txt라는 파일을 하나 더 만든 뒤, 'main 2'라는 내용을 입력하고 저장한다. 이후, 'main work 2'라는 메세지로 커밋한다.

5. o2 브랜치로 체크아웃 한 뒤, o2.txt 파일 생성, 'o2 2' 내용을 저장한다. 이후, 'o2 work 2'라는 메세지로 커밋한다.

6. 로그를 확인해본다.
```
* c1fe335 (HEAD -> o2) o2 work 2
| * 8a0d44a (main) main work 2
|/  
* 19c908d work 1
```

7. o2 브랜치의 작업이 끝났다고 가정하고, main 브랜치로 병해본다.
```
$ git checkout main
$ git merge o2
```
- 브랜치를 병합하려면 먼저 main 브랜치로 체크아웃해야 한다.

8. 로그를 통해 어떻게 병합되었는지 확인한다.
```
*   bc0eddc (HEAD -> main) Merge branch 'o2'
|\  
| * c1fe335 (o2) o2 work 2
* | 8a0d44a main work 2
|/  
* 19c908d work 1
```
- 'o2 work 2' 커밋이 main 브랜치에 병합되면서 'Merge brance o2' 커밋이 새로 생겼다.

**브랜치를 병할할 때 편집기 창이 열리지 않게 하려면**  
```
git merge o2 --no-edit
```
브랜치를 병합할 때 편집기 창이 나타나지 않도록 설정한 경우, 커밋 메세지를 추가하거나 수정하고 싶다면 병합 명령어에 ```--edit``` 옵션을 사용한다.
```
git merge o2 --edit
```