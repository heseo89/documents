# rebase VS Merge

## Merge
1. Merging의 장점
1-1. 이해하기 쉬움
1-2. 원래 branch의 컨텍스트를 유지함
1-3. Fast-Forward Merge를 하지 않는다면 branch별로 커밋을 유지
1-4. 원래 Branch의 커밋들은 변경되지 않고 계속 유지되어 다른 개발자들의 작업과 공유되는것에 대해 신경 쓸 필요가 없음

2. Merging의 단점
2-1. 단순히 모든 사람들이 같은 branch에서 작업하기 떄문에 merge해야할 때에는  Merge가 commit history상으로 전혀 유용하지 않고 복잡함.

## rebase
1. Rebase의 장점
1-1. 단순한 history
1-2. 여러 개발자들이 같은 branch를 공유할 떄 커밋을 합치는 가장 직관적이고 깔끔한 방법.

2. Rebase의 단점
2-1. 충돌상황에서 복잡. 커밋 순서대로 Rebase를 하는데, 각 커밋마다 충돌해소를 순서대로 해주어야 함.
2-2. 해당 커밋들을 다른  곳에 push한 적이 있다면 history를 다시 쓰는것에 부작용이 발생.


# 활용 방법
* 여러 개발자들이 같은 branch를 공유할 때는 Pull & Rebase가 history를 깔끔하게 유지하기 좋음
* 완료된 기능 branch를 다시 합칠때는 Merge가 좋음
* 기능 branch에 parent branch의 변경 내용을 반영하고 싶을 때는 Rebase가 좋음.


# Branch 전략
* master - 최종 릴리즈한 안정화된 버전
* develop -다음 release를 위한 개발중인 최신 빌드
* feature - 기능 breanch      :: develop branch에서 가져오며, 다시 develop branch로 Merge
* release - 릴리즈 점검을 위한 branch    :: develop branch에서 가져오며, develop, master branch로 Merge
* hotfix - 긴급 버그 픽스를 위한 branch    :: master branch에서 가져오며, master, develop branch로 Merge

# Portal의 Git flow
* CM 시
>* release branch를 master, develop branch에 Merge. masterbranch에 tagging
* 개발 시
>* 모든 feature branch는 develop branch에서 가져온다. (parent branch)
>* (origin) feature branch 개발 완료 후에  (upstream) feature branch에 Merge 후, (upstream)의 develop branch에 Merge :: 직후 release에 포함될 경우에만
* QA 시
>* release branch는 develop branch에서 가져오며 통합 테스트 시에 사용
>* 각 feature branch 테스트 시에는 다른 방법을 생각해야 함.
* hotfix가 있을 경우
>* release branch가 master branch에 merge 되어있을 경우.
>>* hotfix branch는 master branch에서 가져오고, 작업 후 master, develop branch에 Merge
>* release branch가 master branch에 merge 되어있지 않을 경우
>>* CM에 나가 release branch에서 hotfix branch생성 후,  해당 release branch에만 Merge


**매일 아침 작업중인 feature branch에 develop branch를 PULL, release branch에 develop branch를 merge.**
