# 팀 코딩 컨벤션 정립하기

### 코딩 컨벤션?

> 읽고, 관리하기 쉬운 코드를 작성하기 위한 일종의 코딩 스타일 규약이다.

개발자들 사이에서 100년 전쟁이라 불리는, 긴 역사를 가진 논쟁거리는 한 두 개가 아니다.

1. 탭 vs 스페이스
2. 스페이스 2칸 vs 4칸
3. 세미콜론(;)의 유무
4. `if`, `for`, `while` 후 괄호의 위치
5. 변수 선언 시 `final`의 유무

언급한 것들보다도 훨씬 많겠지만, 이렇게 일반인들이 보면 사소할 수 있는 문제들에 대해 개발자들은 항상 논쟁해왔다. 하지만 어떤 이런 논쟁에서 진영이던 공동의 적은 정해져있었다.

![탭이냐 스페이스냐보다는 일관성이 중요하다.](../.gitbook/assets/200626-untitled-0.png)

왜 한낱 텍스트에 불과한 코드를 짜는데 일관성이나 규칙이 필요할까?

#### 코딩 컨벤션은 필요 없을 수도 있다.

다음과 같은 상황이라면 코딩 컨벤션이 필요 없을 수도 있다.

* 혼자서 개발한다.
* 한 달 전에 내가 짠 코드를 보더라도 한 번에 파악할 수 있다.
* 다른 누군가가 쓴 코드를 보더라도 한 번에 파악할 수 있다.

아마 이 글을 읽고 있는 독자라면 이런 경우는 극히 드물 것이라고 생각한다.

#### 코딩 컨벤션이 있다면?

* 코드 구조의 일관성을 지킬 수 있다.
* 다른 팀원과의 소통 또는 유지보수 시 코드를 이해하는 데 있어 많은 시간을 아낄 수 있다.
* _팀 또는 회사를 나가더라도 그들이 나에게 도와달라는 부탁을 하지 않는다._
* _나 역시 새로운 팀 또는 회사에 들어가더라도 전임자에게 쓸데없는 연락을 하지 않아도 된다._

#### 그럼 코딩 컨벤션을 어떻게 만들지?

1. 기존의 팀 또는 부서에 컨벤션이 있다면 따르면 된다.
2. 없다면 팀원들과 협의하에 만들면 된다.
3. 새로 만드는 것이 두렵다면 **남들이 충분히 고민하고 만들어 놓은 것들을 사용하자.**

Python의 경우에는 커뮤니티 차원에서 만들어 낸 [PEP8](http://www.python.org/dev/peps/pep-0008/)이 있고, Java의 경우에는 [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) 등이 유명하다. 이렇게 커뮤니티나 큰 기업에서 만들어 놓은 컨벤션을 베이스로 팀원들과 협의하며 팀의 스타일에 맞게 조금씩 수정하면 된다. 본인이 제일 좋아하는 언어나 프레임워크 뒤에 Convention을 붙여 구글에서 검색해보면 쉽게 찾을 수 있다.

### 코딩 컨벤션을 쉽게 적용하는 방법

서론이 길었는데, 이 글에서는 코딩 컨벤션을 어떻게 쉽게 적용할 수 있을지에 설명하기 위한 글이다. 나는 이번에 팀원들과 하는 프로젝트에서 Java와 JavaScript를 사용하여 웹 기반 프로젝트를 만들기로 했다. 우선 큼지막하게 베이스가 될 유명한 컨벤션들을 찾아보았다.

* Java
  * Google - [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
  * Oracle - [Java Code Conventions](https://www.oracle.com/technetwork/java/codeconventions-150003.pdf)
  * 네이버 - [**캠퍼스 핵데이 Java 코딩 컨벤션**](https://naver.github.io/hackday-conventions-java/)
* JavaScript
  * Google - [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
  * Airbnb - [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) ([한글판](https://github.com/tipjs/javascript-style-guide))
  * TOAST UI - [코딩 컨벤션](https://ui.toast.com/fe-guide/ko\_CODING-CONVENSION/)
  * 네이버 - [**Naver JavaScript Style Guide**](https://github.com/naver/eslint-config-naver/blob/master/STYLE\_GUIDE.md)

아직 팀 차원에서 정해진 것은 없지만, 우선 컨벤션을 적용해보는 것이 목표이므로 Java와 JavaScript 모두 네이버의 컨벤션을 베이스로 적용해보자. **물론, 적용하기에 앞서 베이스가 될 코딩 컨벤션은 반드시 다 같이 한 번 이상 정독하도록 하자.**

#### Java: IntelliJ Code Style

IntelliJ IDEA 뿐 아니라 JetBrain 계열 제품군들은 코드 스타일을 XML를 통해 쉽게 적용할 수 있게 도와준다. 다른 에디터나 IDE에서의 적용방법은 상이하니, 직접 찾아보도록 하자!

**적용방법**

1. 캠퍼스 핵데이 Java 코딩 컨벤션의 [IntelliJ Formatter XML](https://raw.githubusercontent.com/naver/hackday-conventions-java/master/rule-config/naver-intellij-formatter.xml) 파일을 다운받는다.
2. IntelliJ IDEA에서 `Preferences > Editor > Code Style > Java`로 들어간다.
3. 상단 Scheme 오른쪽 톱니바퀴 버튼을 눌러 `Import Scheme > IntelliJ IDEA code style XML` 를 클릭하여 다운 받은 XML 파일을 불러온다. 불러올 때 이름은 `Naver-coding-convention`으로 설정했다.

**커스터마이징**

* Scheme을 적용하면 그 값들이 자동으로 들어오는데, 나같은 경우 indent에서 탭보다는 스페이스를 선호하기 때문에 `Tabs and Indents`의 `Use tab chracter`을 해제하고 `Tab size`를 4로 설정했다.
* 다른 부분들도 (팀원과의 협의 후) 취향에 맞춰 변경할 수 있다.

#### Java: CheckStyle

[CheckStyle](https://checkstyle.sourceforge.io/)은 Java 코드를 작성 할 때 규칙(이 경우 코딩 컨벤션)들을 선언해두고 이를 어길 시 에러 또는 경고를 뿜어내어 개발자로 하여금 컨벤션을 강제로 지킬 수 있도록 도와주는 코딩 컨벤션 검사 도구다. 네이버 핵데이 Java 코딩 컨벤션을 적용할 시 다음을 제외한 나머지들에 대한 컨벤션을 검사할 수 있다.

* [한국어 발음대로의 표기 금지](https://naver.github.io/hackday-conventions-java/#avoid-korean-pronounce)
* [클래스 이름에 명사 사용](https://naver.github.io/hackday-conventions-java/#class-noun)
* [인터페이스 이름에 명사/형용사 사용](https://naver.github.io/hackday-conventions-java/#interface-noun-adj)
* [메서드 이름은 동사/전치사로 시작](https://naver.github.io/hackday-conventions-java/#method-verb-preposition)
* [테스트 클래스는 'Test’로 끝남](https://naver.github.io/hackday-conventions-java/#test-class-suffix)
* [대괄호 뒤에 공백 삽입](https://naver.github.io/hackday-conventions-java/#space-after-bracket)
* [주석문 기호 전후의 공백 삽입](https://naver.github.io/hackday-conventions-java/#space-around-comment)

**적용방법**

CheckStyle은 별도로 실행할 수 있지만, 우선 IDE와 쉽게 연동되도록 [CheckStyle-IDEA](https://plugins.jetbrains.com/plugin/1065-checkstyle-idea) 플러그인을 통해 적용해보자.

1. [Naver CheckStyle Rules XML](https://raw.githubusercontent.com/naver/hackday-conventions-java/master/rule-config/naver-checkstyle-rules.xml) 파일을 다운받는다. 나는 프로젝트 최상단(`$rootDir`)에 이를 추가했다.
2. 우선 나는 다른 규칙을 덮어쓸 생각이 없어서 [여기](https://naver.github.io/hackday-conventions-java/#checkstyle)를 참고하여 XML 파일에서 다음을 삭제했다. 이를 삭제하지 않으면 다음 단계에서 `suppressionFile` 변수에 파일을 할당해야한다.

```markup
​<module name="SuppressionFilter">
   ​<property name="file" value="${suppressionFile}"/>
   ​<property name="optional" value="false"/>
</module>
```

**IDE에 적용하기**

1. `File > Settings > Plugins` 메뉴에서 CheckStyle-IDEA 플러그인을 검색 후 설치한다.
2. `Tools > CheckStyle`에서 `Checkstyle version`을 8.24 이상, `Scan Scope`를 `All sources (including tests)` 로 설정한 후 `Configuration file`에 다운받고 수정한 XML 파일을 추가하자.
3. 이제 우리가 설정한 코딩 컨벤션을 어기는 코드를 작성하면 다음과 같이 경고 표시가 나타난다.

![](../.gitbook/assets/\_2020-06-23\_19.26.10.png)

**Gradle에 적용하기**

CheckStyle은 IDE뿐 아니라 Gradle에서의 설정을 통해 `build` 시 테스트를 돌리듯 코딩 컨벤션을 어긴 코드를 찾아줄 수 있다.

1.  `build.gradle`에 다음과 같이 설정한다.

    ```groovy
    plugins {
        id 'checkstyle'
    }

    ...

    compileJava.options.encoding = 'UTF-8' // UTF-8 설정
    compileTestJava.options.encoding = 'UTF-8'

    ...

    checkstyle {
        maxWarnings = 0 // 규칙이 어긋나는 코드가 하나라도 있을 경우 빌드 fail을 내고 싶다면 이 선언을 추가한다.
        configFile = file("${rootDir}/naver-chestyle-rules.xml") // XML 파일 경로
        toolVersion = '8.27'  // checkstyle 버전 8.24 이상 선언
    }
    ```
2. 위의 코드를 적용한 경우 이제 `build` 시 컨벤션을 어긋나는 코드가 하나라도 있을 경우 `build` 자체를 중지할 것이다. 극단적인 방법일 수 있지만, 코드의 품질을 생각한다면 한 번쯤 팀원들과 논의해볼 수 있을 것 같다.

![](../.gitbook/assets/\_2020-06-23\_19.36.19.png)

#### JavaScript: ESLint

> `node.js`가 설치되어있다는 전제로 진행한다. `node.js` 버전은 LTS(2020년 6월 23일 기준 v12.18.1)를 사용하는 것을 추천한다.

Java에 CheckStyle이 있다면, JavaScript에는 ESLint가 있다. 이 역시 코드의 품질을 위한 정적 코드 분석 도구로, 코딩 컨벤션을 어긴 코드를 찾아준다. 프론트엔드 라이브러리를 사용하는 경우 `vue-cli`에서는 프로젝트 생성 시 ESLint를 설치할 수 있으며, `create-react-app` 역시 이를 자동으로 설치해준다. 이 경우 3번부터 진행하면 된다.

**적용방법**

1. 프론트엔드 프로젝트 최상단에서 `npm install eslint eslint-config-naver --save-dev`를 통해 ESLint와 네이버의 ESLint 컨벤션을 설치한다.
2.  `npx eslint --init`을 진행한다.

    ```bash
    # 코딩 컨벤션을 얼마나 강제할 것인지
    ? How would you like to use ESLint?
      To check syntax only
      To check syntax and find problems
    ❯ To check syntax, find problems, and enforce code style

    ? What type of modules does your project use?
    ❯ JavaScript modules (import/export)
      CommonJS (require/exports)
      None of these

    # React나 Vue를 사용하지 않는다는 것을 전제로 진행 (사용한다면 3번으로)
    ? Which framework does your project use?
      React
      Vue.js
    ❯ None of these

    # TypeScript 사용 시 설정
    ? Does your project use TypeScript? › No / Yes

    # 작동 환경 설정
    ? Where does your code run?
    ✔ Browser
      Node

    ? How would you like to define a style for your project?
      Use a popular style guide           # google, airbnb 등 preset을 사용 가능
    ❯ Answer questions about your style   # eslint:recommended + 개인 취향
      Inspect your JavaScript file(s)

    # eslintrc 파일 형식. JSON을 추천.
    ? What format do you want your config file to be in?
      JavaScript
      YAML
    ❯ JSON

    # 이후는 취향에 따라 진행
    ```
3.  이렇게 진행하면 다음과 같은 `.eslintrc.json` 파일이 생긴다. `vue-cli`나 `create-react-app`의 경우는 `pacakge.json`에 `eslintConfig`라는 객체 내부에 유사한 내용들이 존재할 것이다.

    ```javascript
    {
      "env": {
        "browser": true,
        "es2020": true
      },

      // 어떤 preset을 확장할지 결정할 수 있다. 배열로 받으면 여러 개를 받을 수 있다.
      "extends": "eslint:recommended",

      "parserOptions": {
        "ecmaVersion": 11,
        "sourceType": "module"
      },

      // preset의 규칙들을 덮어쓸 수 있다.
      // 배열의 첫번째는 [어길 시 경고정도]이며 두번째는 [내용]이다.
      "rules": {
        "indent": ["error", 2],
        "linebreak-style": ["error", "unix"],
        "quotes": ["error", "double"],
        "semi": ["error", "always"]
      }
    }
    ```
4. 나머지는 냅두고, `extends`와 `rules`만 조정해보자. `extends`를 지운 후 `"naver"`로 대체하자. 기존의 `preset`을 사용하지 않겠다면 기존의 `preset`은 삭제해도 무방하다. 그리고 JS 코드를 작성하면 다음과 같이 규칙을 어기는 것들에 대해 경고 또는 에러 표시가 뜰 것이다.

![](../.gitbook/assets/\_2020-06-24\_15.46.20.png)

![](../.gitbook/assets/\_2020-06-24\_15.47.45.png)

> IntelliJ의 경우 `Languages & Frameworks > JavaScript > Code Quality Tools > ESLint` 의 설정에서 Run eslint —fix on save를 선택하면 파일 저장 시 코딩 컨벤션을 어긴 부분 중 수정 가능한 부분을 수정해준다.

**커스터마이징**

1.  설정된 규칙들을 수정하고 싶을 수 있다. 예를 들어 나는 `console.log`를 사용해야하기 때문에 위 사진에서 나온 `no-console`이라는 규칙에 대한 경고 표시를 없애고 싶다. 이 때는 다시 `.eslintrc` 또는 `package.json`의 `eslintConfig`에서 `rules`를 수정하면 된다.

    ```javascript
    "rules": {
        // ...
            "no-console": "off"
    }
    ```

    이 경우 `no-console` 키를 만들고 그 값에 `off`를 넣으면 되는데, ESLint의 `rules`를 설정할 때는 이렇게 문자열로 `off`, `warn`, `error` 중 하나를 선택해 규칙의 강도를 설정할 수 있다. 배열로 넣는 경우 두번째 인자로는 그 특정 규칙에 대한 값을 넣게 되는데, 그 값에 무엇이 들어가는지는 [ESLint 문서](https://eslint.org/docs/rules/)에서 확인해보자.
2.  ESLint에서 공식적으로 지원하지는 않지만 다른 사용자들이 만든 규칙을 플러그인 형식으로 가져올 수 있다. 예를 들어, 우아한테크코스의 프론트엔드 개발 제약사항 중 **`for`문 사용 자제하기** 가 존재하는데, 이에 해당하는 규칙을 적용해보자. 구글링을 해보니 [eslint-plugin-no-loops](https://github.com/buildo/eslint-plugin-no-loops)가 있길래 적용해보기로 했다. `npm install --save-dev eslint-plugin-no-loops`를 통해 플러그인을 설치 후 설정 파일에 다음을 추가한다.

    ```javascript
    "plugins": ["no-loops"],
    "rules": {
        "no-loops/no-loops": "warn"
    }
    ```

![](../.gitbook/assets/\_2020-06-24\_16.02.40.png)

플러그인으로 가져온 규칙이 성공적으로 적용된 것을 확인할 수 있다. 다양한 커스텀 규칙들은 [awesome-eslint](https://github.com/dustinspecker/awesome-eslint)에서 확인 할 수 있다.

#### 공통: [EditorConfig](https://editorconfig.org/)

팀원들마다 다른 에디터를 쓰는 경우도 있고, 같은 에디터를 쓰더라도 포맷팅 규칙을 다양하게 설정하는 경우가 있다. 프로젝트마다 다른 팀원들과 하는 경우 매번 설정을 바꾸는 것도 번거롭다. `.editorconfig` 는 다양한 에디터와 IDE에서 공통적으로 지원하는 코드 스타일에 대한 설정 파일이다. 가급적 이 파일을 프로젝트 최상단에 저장해두고 Git을 통해 팀원들과 공유하는 것을 권장한다. 다음은 네이버 캠퍼스 핵데이 Java 코딩 컨벤션에서 제공하는 예시다.

```javascript
# top-most EditorConfig file
root = true

[*]
# [encoding-utf8]
charset = utf-8

# [newline-lf]
end_of_line = lf

# [newline-eof]
insert_final_newline = true

[*.bat]
end_of_line = crlf

[*.java]
# [indentation-tab]
indent_style = tab

# [4-spaces-tab]
indent_size = 4
tab_width = 4

# [no-trailing-spaces]
trim_trailing_whitespace = true

[line-length-120]
max_line_length = 120
```

**Gradle 설정**

`editorconfig-gradle-plugin`을 설정하면 `.editorconfig` 의 선언과 어긋나는 파일이 존재하면 빌드를 실패하게 만들수 있다.

```groovy
plugins {
    id 'org.ec4j.editorconfig' version '0.0.3'

}
editorconfig {
    excludes = ['build']
}

check.dependsOn editorconfigCheck
```

### 우리는 함께 일한다

![7월부터 매일같이 할 코드 리뷰가 기대된다.](../.gitbook/assets/200626-untitled-2.png)

혼자 모든 것을 만들어내는 사람보다는 함께 만드는 사람들이 더 많을 것이다. 그래서 프로젝트를 진행하며 서로 코드 리뷰를 하고, 함께 고민하고, 서로 부족한 부분을 채워주며 발전한다. 코딩 컨벤션은 팀의 원활한 소통과 이해를 위해 만드는 것이지만, "우리"가 더 대단한 것들을 해내기 위해 쓸데없는 갈등이나 고민을 하지 않게 해주는 시스템이라고 생각한다. 이런 시스템이 잘 정립된다면 구성원들 역시 개발자로서, 팀 플레이어로서 훨씬 많이 성장할 수 있지 않을까?
