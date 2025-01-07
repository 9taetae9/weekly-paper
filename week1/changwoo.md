
<details>
<summary>1주차</summary></summary>
<div markdown="1">

- 🇶 git rebase와 git merge의 차이점을 설명하고, 각각 어떤 상황에서 사용하는 것이 적절한지 설명해주세요.
  - git rebase : 두 개의 공통 base를 가진 git rebase - 두 개의 공통 base를 가진 branch에서 한 branch의 base를 다른 branch의 최신 commit으로 branch의 base를 옮기는 작업
    - 장점 : 공유 branch의 최신 변경사항을 즉각 반영할 수 있다.
    - 단점 :  branch의 각각의 commit마다 conflict처리
   - git merge : 브랜치를 만들어서 원하는 기능을 만들고 이상없이 작동한다면 원래의 브랜치로 다시 합쳐줘야 한다. 브랜치와 브랜치를 합치는 과정
   - 차이점 :  merge는 공유 branch에 대한 변경사항을 즉각 대응하기 어렵다. rebase를 사용한다면, 동료 개발자들이 올린 commit들의 수정사항을 내가 작업하고 있는 branch에 즉각 반영
   
   - commit log를 어떻게 관리하고 git을 어떻게 사용하는지에 따라 목적에 맞게 사용

  ---
- 🇶 git fetch와 git pull의 차이점을 설명하고, 각각을 사용하는 것이 적절한 상황을 설명해주세요.

</div>
</details>

