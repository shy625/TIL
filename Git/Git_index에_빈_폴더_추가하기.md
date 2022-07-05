# Git index에 빈 폴더 추가하기

프로젝트를 진행하다 보면 때때로 빈 폴더가 필요한 경우가 있다. 프로젝트 진행 전 폴더 구조를 미리 잡거나, `uploads/`와 같은 자원 관리용 폴더를 만드는 경우 등이다. 문제는 Git index (staging area)에서는 파일만 관리한다는 것이다. 때문에 파일이 들어있지 않은 빈 폴더는 `git add`를 하더라도 index에 추가되지 않고, `commit`과 `push` 또한 할 수 없다. 이럴 때 `.gitkeep` 파일을 사용하면 빈 폴더를 Git index에 추가할 수 있다.

## .gitkeep

`.gitkeep` 파일은 일종의 더미 파일이다.

파일이 없는 빈 폴더는 index에 추가할 수 없기 때문에 빈 폴더 대신 폴더 안에 `.gitkeep`이라는 더미 파일을 만든 후 index에 추가하는 것이다. 이렇게 하면 폴더 안에 파일이 있기 때문에 해당 파일이 등록되면서 폴더도 같이 등록되게 된다.

사실 `.gitkeep` 파일은 Git에서 공식으로 제공하는 기능이 아니다. 따라서 꼭 `.gitkeep` 파일이 아니더라도 일반 텍스트 파일이든, 어떤 이름의 파일이든 상관없이 임의의 더미 파일을 생성한 뒤 폴더를 등록하는 것도 가능하다. 다만 `.git` 이나 `.gitignore`같이 Git에서 공식으로 사용하는 파일명과 비슷하고, 관례적으로 많이 사용하고 있어 보통 `.gitkeep` 이라는 파일을 사용한다. (`.gitkeep` 대신 `.gitignore` 파일을 내용없이 생성하여 추가하는 방법도 있지만 `.gitignore` 본래의 역할과 혼동될 가능성이 있어 좋은 방법은 아닌 듯하다.)

## 예시

```bash
# 1. 추가하고자 하는 빈 폴더 생성
mkdir empty_dir

# 2. 추가한 빈 폴더 안에 들어가서 .gitkeep 파일 생성
cd empty_dir
touch .gitkeep

# 3. git add 하여 Git index에 등록
git add .

# 4. commit, push 등 Git을 이용해 자유롭게 폴더 관리
git commit -m "Add empty directory"
git push origin
```

## 참고

- [Use .gitkeep to commit & push an empty Git folder or directory](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/gitkeep-push-empty-folders-git-commit)
- [제타위키 - .gitkeep 파일](https://zetawiki.com/wiki/.gitkeep_%ED%8C%8C%EC%9D%BC)
- [Can I add empty directories?](https://git.wiki.kernel.org/index.php/Git_FAQ#Can_I_add_empty_directories.3F)
- [What is .gitkeep? How to Track and Push Empty Folders in Git](https://www.freecodecamp.org/news/what-is-gitkeep/)
