## 🇶 git rebase와 git merge의 차이점을 설명하고, 각각 어떤 상황에서 사용하는 것이 적절한지 설명해주세요.
### rebase
-  두개의 branch를 하나로 합치는 작업방법
- 하나의 branch를 다른 하나의 branch의 base로 맞추는것
- git rebase는 말 그대로 branch의 base를 현재 base로 맞추는것
- commit history를 깔끔하게 하나로 관리
- main branch가 수정 되었을 때 내 작업 branch를 main branch로 rebase하여 main에서 처음부터 작업한 것 처럼 할수있음
- 이력관리가 잘되고 직관적이고 시간의 순서대로 하나의 흐름으로 이해할 수 있어서 관리하기 편함
### merge
- merge는 뜻처럼 병합임
- 하나의 branch의 수정사항이 main branch로 병합되면서 쌓여나가는 방식
- 작업 branch에서 수정하는 동안 main에서 수정이 발생해서 두 개가 달라진 경우에 main에서 merge를 수행하면 새로운 merge commit이 생성되면서 작업 branch의 내용을 origin 위에 쌓게 됨
- 새로운 merge commit을 통해 두 개의 branch가 쉽게 합쳐진다는 것이고 방식 자체가 Rebase에 비해 직관적임
- 기존의 반영한 commit들이 유지됨
- 전체적인 merge commit으로 인해 history가 복잡해짐
### 차이점
- rebase는 commit history를 재작성하여 선형적으로 만들며 깔끔한 로그를 남김
- branch는 merge는 branch 전체 히스토리를 유지하며 branch가 병합된 전체적인 로그를 남김
- rebase는 conflict를 하나씩 해결해 나가야함
- merge는 히스토리 보존이 중요할 때 사용하고 rebase는 깔끔한 히스토리가 필요할 때 사용됨

  
## 🇶 git fetch와 git pull의 차이점을 설명하고, 각각을 사용하는 것이 적절한 상황을 설명해주세요.
### fetch
- git fetch는 원격 저장소의 최신 변경사항을 다운로드하지만, 로컬 작업 디렉토리에는 영향을 주지 않음(병합x)
- 원격 저장소의 변경사항을 안전하게 확인할 수 있음
- 로컬 git 저장소만 업데이트하고 작업 디렉토리는 변경하지 않으며, 변경사항을 검토한 후 수동으로 병합해야 함
- branch는 FETCH_HEAD 의 이름으로 체크아웃이 가능함
### pull
- git pull은 원격 저장소의 변경사항을 가져와 현재 브랜치에 자동으로 병합함
- 원격 저장소의 변경사항을 즉시 로컬 브랜치에 통합함 git fetch와 git merge를 연속으로 수행하는 것과 동일함
- 작업 디렉토리의 파일을 직접 업데이트함
- git fetch는 원격 변경사항을 안전하게 검토하고 싶거나 현재 작업 중인 파일에 영향을 주지 않고 원격 변경사항을 확인하고 싶을 때 사용함
- 원격 변경사항을 빠르게 로컬 브랜치에 통합하고 싶거나 작업 디렉토리를 최신 상태로 즉시 업데이트하고 싶을 때 사용함
