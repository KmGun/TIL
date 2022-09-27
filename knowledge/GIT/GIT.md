# GIT

# 220927 - 코딩애플로 GIT 복기

# add, commit, log, difftool

- git log 옵션들
    - —all —online :
    - -p :
- git difftool : git diff는 쓰레기.
    - git difftool 커밋아이디1 : 현재와 1 비교
    - git difftool 커밋아이디1 커밋아이디2 : 1 2 비교

# checkout vs switch ,restore

→ checkout이 switch와 restore으로 쪼개짐. checkout 쓰지말자

# git branch,merge

- 사용법
    
    ```
    // 브렌치 생성,이동
    git branch 새브렌치
    git swtich 새브렌치
    
    // merge
    git switch main
    git merge 내꺼에 병합시킬 대상브렌치
    
    // merge 삭제
    git branch -d/-D : merge 완료 브렌치 삭제 / merge 안한 브렌치 삭제
    ```
    
- 기타
    - HEAD : 그냥 우리 자신을 의미
    - main / master = 메인 브렌치
- merge 방식들
    - **3-way merge**
        
        → main , 새브렌치에 commit이 존재 할 경우
        
    - **fast-foward-merge**
        
        → main 브렌치에만 신규 커밋이 없는경우 자동 발동 === 굳이 메인 브렌치를 살려두지 않고, 새로 커밋한 브렌치를 main으로 자동설정
        
        ++ git merge —no—ff : 자동 발동 안되게
        
    - **rebase**
        
        → 브렌치시작점을 옮기는것
        
        ![스크린샷 2022-09-27 오후 9.07.11.png](GIT%20975bba27ae6e4e229cf0efcab06e9351/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_9.07.11.png)
        
        ![스크린샷 2022-09-27 오후 9.08.08.png](GIT%20975bba27ae6e4e229cf0efcab06e9351/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_9.08.08.png)
        
    - 목적
        - git log가 너무 복잡해지는것 방지
    - 사용법
        1. git switch 새브렌치
        2. git rebase main
        3. git checkout main
        4. git merge 새브렌치 (fast-foward-merge 발동 시키는거임.)
- merge-squash
    
    → 자잘한 branch들의 commit내용을 하나의 새로운 커밋(commit 4)으로 정리해서 바꿔줌
    
    ![스크린샷 2022-09-27 오후 9.16.57.png](GIT%20975bba27ae6e4e229cf0efcab06e9351/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_9.16.57.png)
    

# git revert / reset /restore

- git restore
    
    → 옵션을 준 쪽으로, 로컬 파일이 되돌아감 === 커맨드 z 의 git 버전 (커밋엔 반영안됨)
    
    - git restore 파일명
        
        → 최근 커밋으로 그 파일을 되돌림
        
    - git restore —source 커밋아이디 파일명
        
        → 특정 커밋으로 “”
        
    - git restore —staged 파일명
        
        → git add 파일명 취소
        
- git revert
    
    → 그 커밋의 내용만 취소시킨 내용의 커밋을 새로 생성해줌
    
    ++ git에선 아예 과거 삭제는 불가능.
    
    - 사용법
        - git revert 커밋아이디
        - git revert 커밋아이디1 커밋아이디2
        - git revert HEAD/최근커밋아이디
- git reset —hard
    
    → 커밋내용을 아예 삭제
    
    ** 협업 할땐 잘안씀
    
    ** —soft / —mixed
    

# 협업

## push, pull

- github는 기본 브렌치를 main으로 하라고 강요함.
    
    → git branch -M main : 기본 브렌치 이름 변경방법
    
- git push
    - 사용법: git push -u 원격저장소명/주소 **푸시할브렌치명**
        - -u : 뒤의 두옵션 기억한다는 의미
            
            → git push / git pull만 해도 되게 됨.
            
        
        **** 특정 브렌치만 push 가능한것에 유의**
        
- git pull
    - 사용법 : git pull 원격저장소명/주소 푸시할브렌치명
        - push 할떄 -u 달아놓았으면, git pull 만 쳐도됨
        
        **** 특정 브렌치만 pull 가능한것에 유의**
        
    
    ** git pull에 = git fetch + git merge
    
    → pull을 처음에 안하고 작업하다 pull 할시 , merge conlfict 일어날수도 있다.
    

## step by step

1. 브렌치 파기
    
    →깃헙에서 만들거나 / 로컬에서 만들어서 푸시하거나
    
2. 브렌치 작업하고 , 브렌치 단위마다 푸쉬
3. pull request로 확인하고, main에 merge

# git stash

→ commit 전에 코드를 따로 저장 해놓는 방법.

** 사실 브렌치 만드는거랑 다를바가 별로없음