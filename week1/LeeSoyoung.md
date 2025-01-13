# 목차

[1. git merge / git rebase](#git-rebase와-git-merge의-차이점을-설명하고-각각-어떤-상황에서-사용하는-것이-적절한지-설명해주세요)

[1-1. git merge](#git-merge)

[1-2. git rebase](#git-rebase)

[1-3. 차이점](#차이점)

[1-4. 요약](#요약)
  
[2. git pull / git fetch](#git-fetch와-git-pull의-차이점을-설명하고-각각을-사용하는-것이-적절한-상황을-설명해주세요)

[2-1. git pull](#git-pull)

[2-2. git fetch](#git-fetch)

[2-3. 요약](#차이점-요약)

# git rebase와 git merge의 차이점을 설명하고, 각각 어떤 상황에서 사용하는 것이 적절한지 설명해주세요.

## git merge
```git merge```란?

말 그대로 **합치는** 것이다. 보통 머지를 쓰는 경우 다음과 같다.

0. develop 브랜치로부터 feature1, feature2 브랜치를 생성, 분기한다.
1. feature1, feature2 브랜치에서 각자 작업한다.
2. 작업이 완료되어 feature1 작업 내용을 develop 브랜치로 합친다.
3. feature2 작업 내용을 develop 브랜치로 합친다.


>
참고한 git 문서에서는 각 커밋이 원래 내용에 합쳐진다는 의미로
화살표가 이전 커밋을 향하지만
시간의 흐름에 따라 생각해보고 싶어서 내 그림에서는 다음 커밋을 향한다.

현재 원격 저장소에 커밋이 다음과 같이 존재하고, develop 브랜치는 가장 최신 커밋에 있다. 
![](https://velog.velcdn.com/images/soo7132/post/6b56ba5e-64f6-4574-ad33-eb1937a6373b/image.png)


develop 브랜치로부터 feature1, feature2 브랜치를 만든다.
``` 
$ git checkout -b feature1
$ git checkout -b feature2
```
![](https://velog.velcdn.com/images/soo7132/post/c0af7d73-bbd6-4ed6-9512-6ba1f0287a02/image.png)

feature1, feature2 브랜치에서 각각 작업을 하고 커밋을 한다. develop 브랜치에서보다 더 많은 작업을 하고 커밋을 했기 때문에 feature 브랜치들은 앞으로 나아간다.
```
$ git commit -m "feature1"
$ git commit -m "feature2"
```
![](https://velog.velcdn.com/images/soo7132/post/72cd6213-2c4f-4d2a-a2c7-fa11fdb2df10/image.png)

feature1에서 C3 커밋 내용을 develop 브랜치에 합친다.
```
$ git checkout develop
$ git merge feature1
```

**Fast forward**
feature1 브랜치는 develop이 가리키던 C2 커밋에서 기반하였기 때문에 develop 브랜치에서는 단순히 C3 커밋 내용을 추가하기만 하면 되므로, develop 브랜치 포인터는 feature1과 같이 C3 커밋을 가리킨다. 이렇게 단순히 앞으로 이동하는 것을 Fast forward 방식이라고 한다.
![](https://velog.velcdn.com/images/soo7132/post/678c8b03-770b-453f-ad33-9d1627484013/image.png)

feature2 브랜치에서 추가 작업을 하고, 커밋을 해도
develop, feature1과는 전혀 관계가 없다.
```
$ git checkout feature2
$ git commit -m "feature2:C5"
```
![](https://velog.velcdn.com/images/soo7132/post/34874f72-06ea-40d1-baa6-4292c370e406/image.png)


이제 feature2 브랜치를 develop에 합쳐보자.
```
$ git checkout develop
$ git merge feature2
```
**3-way Merge**
Fast forward 때처럼 간단히 합칠 순 없다. feature2는 develop과는 다른 히스토리를 쌓았기 때문이다. 이때는 각 브랜치가 가리키는 커밋 두 개와, 두 브랜치의 공통 조상을 이용하여 3-way Merge를 한다.

develop 브랜치의 포인터가 최신 커밋 C5로 옮겨가는 게 아니라, 공통 조상 C2, develop 브랜치의 현재 위치 C4, feature2 브랜치에서 머지하고자 하는 C5, 이 세 커밋을 git에서 적절히 merge하고(깃허브가 알아서 해준다), 이를 성공하면 새로운 별도의 커밋 C6을 만든다.
그리고 develop 브랜치는 이 C6을 가리킨다.
![](https://velog.velcdn.com/images/soo7132/post/8b971402-e86c-4fe9-840f-c173da805299/image.png)


그럼 최종적으로 develop 브랜치에 히스토리를 살펴보면 다음과 같이 된다.
가장 처음 커밋인 C0부터 가장 최신 커밋인 C6까지의 모든 커밋을 다 담고 있다고 보면 된다. 
C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6
![](https://velog.velcdn.com/images/soo7132/post/0843e4ac-a6ae-4eb7-9b64-475706e240bb/image.png)


## git rebase

```git rebase란?```
re"base"라는 이름에서 알 수 있듯 브랜치의 베이스를 새롭게 설정한다는 뜻이다.

- **base**: 특정 브랜치가 생성된 시점의 기준점이 되는 커밋(브랜치의 히스토리 중 가장 먼저 생성된 커밋)

동료 개발자가 공유 브랜치에 새로운 커밋을 올리고, 그것을 내 브랜치에 반영해야 할 때, rebase를 사용한다면 최신 커밋을 브랜치에 반영할 수 있다.

다음과 같은 상황을 가정해보자.
develop 브랜치의 C1 커밋 시점에서 feature1, feature2 브랜치로 각각 분기하여 작업을 이어 나간다.
이때 각 브랜치의 베이스는?
- develop: C0
- feature1: C1
- feature2: C1
![](https://velog.velcdn.com/images/soo7132/post/d5b0b491-8f1d-43e8-96f1-fcbccce13479/image.png)


feature1의 작업이 끝나 develop에 merge하였다. feature1은 develop의 최신 커밋으로부터 이어진 브랜치이므로 Fast forward 방식이다.
```
$ git checkout develop
$ git merge feature1
```
![](https://velog.velcdn.com/images/soo7132/post/c34513f3-58c2-4efa-b85b-3a3810a7699c/image.png)


feature2에서 작업하던 개발자가 새롭게 업데이트된 develop의 내용을 반영하고 싶다면, 즉 feature2가 develop의 커밋을 기반으로 다시 작성되길 원한다면
feature2의 베이스를 develop의 최신 커밋으로 옮기면 된다. "feature2를 develop 위로 올린다."고 생각할 수 있다.

 - feature2의 새로운 베이스: C3
 
```
$ git checkout feature2
$ git rebase develop
```
이러면 merge를 하지 않았는데도, feature2가 develop의 내용을 가지게 된다.
![](https://velog.velcdn.com/images/soo7132/post/4bd67b55-438f-4dc5-bac8-a82e401b6413/image.png)

만약 feature2에서 한 번 더 커밋을 작업을 마무리한 후
develop에 이 작업 내용을 합치고 싶다면 develop에서 merge를 하면 된다. feature2의 베이스가 develop의 최신 커밋이므로 Fast forward 방식이다.
```
$ git checkout develop
$ git merge feature2
```
![](https://velog.velcdn.com/images/soo7132/post/331dcc78-5ec6-45d4-9629-8aaafa96f747/image.png)
이때 merge 대신 물론 rebase를 해도 된다. 이때는 develop의 베이스를 feature2로 하면 된다.

feature2의 커밋 히스토리이다.
베이스였던 C1으로부터 develop의 최신 커밋인 C2, C3을 포함하게 됐다.
![](https://velog.velcdn.com/images/soo7132/post/a8e74dff-b4fe-40d3-9feb-52c26c718b71/image.png)

develop의 커밋 히스토리이다.
feature2가 Fast forward 방식으로 합쳐져 feature2의 최신 커밋으로 옮겨갔다.
![](https://velog.velcdn.com/images/soo7132/post/df51292c-8308-462a-ae1c-1758921e4ad6/image.png)



## 차이점
merge와 rebase를 단순하게 나타내면 아래와 같다. 결과물은 같지만 도달하는 과정이 다르다.
merge는 두 브랜치가 합쳐졌다는 게 명확히 드러나고, rebase는 히스토리가 직렬되어 있다.
![](https://velog.velcdn.com/images/soo7132/post/e100ac99-43eb-441d-b1d9-6abc3376a06b/image.png)

브랜치가 많아지면 많아질수록 그 차이가 더 명확하다.
![](https://velog.velcdn.com/images/soo7132/post/42d36c21-1bc1-4764-a6e1-fa83b2780224/image.png)


1. 3-Way Merge를 기준으로 merge는 새로운 커밋을 만든다. 그러나 rebase는 새로운 커밋을 만들지 않고, 다른 브랜치의 위로 올라간다.
2. 따라서 rebase로 만들어진 커밋 히스토리는 merge로 만들어진 히스토리보다 깔끔하다.

## 요약

```git merge```
- 두 브랜치의 최신 커밋끼리 합쳐져 새로운 커밋을 만든다.
- 두 브랜치를 합쳤다는 정보가 커밋 히스토리에 꼭 남아야하는 경우 merge를 사용한다.

```git rebase```
- rebase를 하려는 브랜치가 base가 될 브랜치의 위로 올라간다.
- 커밋 히스토리를 깔끔하게 유지하는 게 더 중요한 경우 rebase를 사용한다.
- 단, 히스토리를 조작하는 행위이므로 주의해야 한다.
    - 이미 원격에 push한 커밋을 rebase하지 마라.
    - 다른 동료가 작업 중인, 원격 저장소에 공개된 브랜치를 대상으로 리베이스하지 말라는 뜻이다.



---


# git fetch와 git pull의 차이점을 설명하고, 각각을 사용하는 것이 적절한 상황을 설명해주세요.

## git pull
```git pull```은 로컬 레포지토리보다 원격 레포지토리가 더 최신 커밋을 가지고 있을 때, 원격 레포지토리 내용을 로컬 레포지토리에 반영하는 명령어이다.

pull='잡아당기다'로 원격의 내용을 로컬로 잡아당긴다고 볼 수 있다.
동료 개발자와 협업을 하며 코드를 주고 받는 일 중 **받는**일에 해당한다.

원격 레포지토리의 수정 사항을 로컬 레포지토리에 반영하는 것은
**원격 레포지토리의 현재 브랜**치를 **로컬 레포지토리의 현재 브랜치**로 **병합(merge)**하는 것이다. 

### push 전에 pull을 해야하는 경우
내가 로컬 레포지토리에서 내 코드를 수정하는 동안
동료가 원격 레포지토리에 새로운 코드를 올렸다면, ```git push```를 할 수 없다.

이미 동료가 추가한 내용이 있는데 내 코드를 push하면
동료의 내용은 덮어씌워져 사라질 것이다.
그래서 이 경우에는 ```git push```를 시도해도 실패한다.
이때는 push 전에 ```git pull```을 하여 동료의 수정 내용을 내 로컬에 반영하자.
pull은 merge와 같으므로, Merge Conflict가 발생할 수 있다. 해결하여 pull을 마무리한다.

그 이후 push하면 정상적으로 동작한다.

## git fetch
pull은 원격 레포지토리의 수정사항을 가져와서 로컬 레포지토리의 브랜치에 merge를 한다면,
```git fetch```는 가져오기만 하고, merge하지 않는다.

> 왜 알아서 merge까지 해주는 ```git pull```을 쓰지 않고 ```git fetch```를 써야할까?

```git fetch```는 일단 원격 레포지토리에 있는 브랜치에 내용을 가져와서 살펴본 후에 merge하고 싶을 때 사용한다.

만약 원격에 있는 내용이 이상한, 필요 없는 내용일 수도 있다. 협업할 때 있을 수 있는 일이다. _~~(신입 개발자라면 할 수 있는 실수다..)~~_ 
그럴 때는 ```git pull```이 병합을 하기 전에 확인이 필요하다.

```
$ git fetch
PS C:\Users\soo71\github\mergePractice> git fetch
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 934 bytes | 103.00 KiB/s, done.
From github.com:gitSoyoungLee/mergePractice
   09196a7..55531e0  main       -> origin/main
```
fetch로 가져온 후 ```git diff``` 명령어를 통해 원격 레포지토리의 브랜치와 로컬 레포지토리의 브랜치의 차이를 확인할 수 있다.
현재 로컬에는 local.md 파일을 생성하고 커밋한 상태고, 원격에서는 remote.md 파일이 있다.
```
$ git diff main origin/main
diff --git a/local.md b/local.md
deleted file mode 100644
index e69de29..0000000
diff --git a/remote.md b/remote.md
new file mode 100644
index 0000000..9c998f7
--- /dev/null
+++ b/remote.md
@@ -0,0 +1 @@
+remote
```

이때 해결방법은

1. 동료에게 찾아가 수정 후 다시 원격에 push하라고 하기
2. 내가 알아서 해결하고 원격에 push하기

## 차이점 요약

```git pull```

1. 원격 레포지토리의 수정 사항 정보를 로컬 레포지토리로 **가져오고 병합**한다.
2. 원격 레포지토리의 최신 커밋을 **검토하지 않고** 로컬에 반영해야 할 때 사용한다.

```git fetch```

1. 원격 레포지토리의 수정 사항 정보를 **가져오기만** 한다.
2. 원격 레포지토리에서 가져온 브랜치의 내용을 로컬 레포지토리의 브랜치에 병합(merge)하기 전에 **점검**이 필요할 때 사용한다.
3. 원격 레포지토리에 있는 브랜치의 내용과 내가 작성한 코드를 비교해야 할 때 사용한다.




