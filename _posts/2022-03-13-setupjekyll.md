---
title: "백지 상태에서 Jekyll 설치해보기"
date: 2022-03-27
description: 배경지식 하나도 없이 백지 상태에서 jekyll 블로그를 만들어보려고 합니다.
featured_image: "/images/02_Post/04_setupjekyll/setupjekyll-cover-post.png"
---

<!--![](/images/demo/demo-landscape.jpg) -->

## 들어가기 전에.

### 정확한 방법이 아닐 수 있다.

아래에 설명하는 방법이 정확한 설치 방법이 아닐 수 있다. 처음 블로그를 만들고서 새로운 맥북에 jekyll을 설치해야 하는 상황이 생겼는데, 처음 했던 방법으로 똑같이 설치했는데도 오류를 만났다. 오류가 생길 때마다 구글링을 통해서 해결했고, 다른 분들이 작성했던 해결책이 되는 경우가 있었고 실패하는 경우도 있었다.

이런 것처럼 모두 동일한 조건에서 설치하는 게 아니고, 오류도 모두 동일한 조건이 아니라서.. 아래 방법이 정확한 방법이라고 하기에는 조금 무리가 있고 참고를 위해서(?) 작성해 본다.

처음 이런류의 글을 쓰는거라서 사설이 길었다. 내 설치 조건은 아래와 같다. 이 글에서는 블로그 생성을 위한 GitHub 설정 방법은 따로 설명하지 않는다!

- **macOs Big Sur 11.6.3**
- **MacBook Pro (15-inch, 2017)**

## Jekyll 설치해보기

jekyll을 설치하기 위한 명령어는 아래와 같지만, 아쉽게도 아무런 과정없이 아래 명령어만 입력하면 아무일도 일어나지 않는다.

```zsh
$ gem install jekyll bundler
```

### Homebrew, rbevn, ruby 설치

아무런 세팅이 안되어있다고 가정하면, 제일 먼저 맥용 페키지 관리자인 `Homebrew` 를 설치해야 한다. 명령어는 아래와 같다.

```zsh
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

설치가 완료되었다면, `$ brew` 또는 `$ brew --version` 명령어를 통해서 제대로 설치가 되었는지 확인할 수 있다.

```zsh
$ brew --version
Homebrew 3.4.3
Homebrew/homebrew-core (git revision 14e552e7a7f; last commit 2022-03-24)"
```

제대로 설치가 완료되었다면, 이제 `rbevn`을 설치할 차례다. 아래에 명령어를 하나씩 입력해보자.

```zsh
$ brew update
$ brew install rbenv ruby-build
```

이제 기본적으로 `ruby`를 설치할 수 있게 되었다. 최신 버전의 `ruby`를 설치하거나 원하는 버전을 찾아서 설치해보자.
버전은 [이곳](https://www.ruby-lang.org/ko/)에서 확인하거나 명령어를 통해서 확인할 수 있다.

```zsh
$ rbenv install -l
```

설치하고 싶은 버전을 찾았다면 버전을 입력하고 설치해보자! 나는 작성일 기준(2022. 03. 27) 최신 버전으로 설치했다.
혹시 미리 설치를 진행해서 여러 버전이 설치되어 있다면.

`$ rbenv versions` `$ rbenv global 버전` `$ rbenv local 버전` 명령어로 버전을 확인하고 디렉토리 별로 버전을 설정할 수 있다.

```zsh
$ rbenv install 3.1.0
$ rbenv rehash
```

참고로 설치 과정에서 나는 `You don't have write permissions for the /Library/Ruby/Gems/2.3.0 directory.` 오류가 나와서 [이 글](https://jojoldu.tistory.com/288)을 참고해서 `rbenv PATH`를 추가했다.

### jekyll 설치, 블로그 생성하기

이제 드디어 jekyll을 설치 할 수 있는 환경이 되었다. jekyll 설치 명령어를 입력해보자.

```zsh
$ gem install jekyll bundler
```

정상적으로 jekyll이 설치된 것을 확인하였다면 이제 본격적으로 블로그를 만들 수 있다.

```zsh
$ jekyll new 블로그 이름 입력
$ bundle install
```

블로그가 정상적으로 생성되었다면, 본인이 지정한 위치에 jekyll blog 명으로 폴더가 생겼을 것이다.
생성된 폴더 우클릭으로 터미널을 열어서 블로그를 실행할 수 있고 터미널이 편하다면,

```zsh
$ cd 본인 블로그 이름
$ bundle exec jekyll serve
```

명령어로 jekyll blog 디렉토리로 이동해서 로컬 환경에서 블로그를 실행할 수 있다.
이제 블로그를 만들기 위한 기본적인 환경이 만들어졌다.

### 부록

매우 특수한 경우에 `$ bundle exec jekyll serve` 명령어로 로컬에서 블로그를 실행하면 아래의 오류들을 만나는 경우가 있다.

첫 번째 오류는 아래처럼 출력되는데 (너무 길어서 오류 케이스를 보여주는 라인만 작성했다.)

```zsh
.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
```

[GitHub](https://github.com/jekyll/jekyll/issues/8523)이슈에서 해결책을 찾았다.

블로그를 실행하는 디렉토리로 이동한 후 아래 명령어를 입력하면 해결 할 수 있다.

```zsh
$ bundle add webrick
```

두 번째 오류는 조금 수정이 필요한 오류인데, 이것 역시 블로그를 로컬에서 실행하면 아래의 오류를 만날 수 있다.

```zsh
                ------------------------------------------------
  Jekyll 4.2.0   Please append `--trace` to the `serve` command
                 for any additional information or backtrace
```

jekyll 블로그 파일을 보면 `Gemfile`을 찾을 수 있다. jekyll의 실행 버전과 기본적인 환경을 관리하는 파일인데 이곳에
아래처럼 수정해주면 정상적으로 실행할 수 있다.

```zsh
Line 5:
gem "jekyll"
to
gem 'jekyll', github: 'jekyll/jekyll'
```

### 다음 글은?

다음 글을 빠르게 작성할 수 있는 여유가 있을지 모르겠지만, 다음 글은 로컬에 생성된 블로그 파일을 GitHub 레포지터리로 올리는 방법과 매우 기본적인 jekyll 블로그 테마 설정 방법, 튜닝 방법에 대해서 작성할 예정이다!
