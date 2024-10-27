# 접근 제어자 (가시성 변경자)

```kotlin
package dev.hodol.sandbox.base

const val EVERYWHERE = "모든 파일에서 사용 가능"
public const val ALSO_EVERYWHERE = "Kotlin의 기본 접근제어자는 public"
private const val ONLY_IN_SAME_FILE = "같은 파일에서만 사용 가능"
internal const val ONLY_IN_SAME_MODULE = "같은 모듈에서만 사용 가능"

open class Visibility {
    internal val onlyInSameModule = "같은 모듈에서만 사용 가능"
    private fun onlyInSameClass() = "같은 클래스에서만 사용 가능"
    protected fun onlyInInheritedClass() = "하위 클래스에서만 사용 가능"
}
```

* `protected`는 ~~당연히~~ 최상위 선언에서 사용할 수 없다.
* Kotlin에는 `package-private`이 없는데, package를 namespace용으로만 쓰고, 대신 `internal`을 이용해 같은 module에서만 사용할 수 있게 한다.
* Java와 다르게 Kotlin에서는 같은 package 안에서도 `protected` 멤버에 접근할 수 없다.
* 확장 함수에서는 `private`이나 `protected` 멤버에 접근할 수 없다.

> 같은 module이라 함은, 예를 들어 Gradle Multimodule 환경 등에서의 module을 말한다.

```java
// Visibility.java
package dev.hodol.sandbox.base;

import kotlin.Metadata;
import org.jetbrains.annotations.NotNull;

@Metadata(
   mv = {1, 4, 3},
   bv = {1, 0, 3},
   k = 1,
   d1 = {"\u0000\u0014\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0002\b\u0002\n\u0002\u0010\u000e\n\u0002\b\u0005\b\u0016\u0018\u00002\u00020\u0001B\u0005¢\u0006\u0002\u0010\u0002J\b\u0010\u0007\u001a\u00020\u0004H\u0004J\b\u0010\b\u001a\u00020\u0004H\u0002R\u0014\u0010\u0003\u001a\u00020\u0004X\u0080D¢\u0006\b\n\u0000\u001a\u0004\b\u0005\u0010\u0006¨\u0006\t"},
   d2 = {"Ldev/hodol/sandbox/base/Visibility;", "", "()V", "onlyInSameModule", "", "getOnlyInSameModule$sandbox_base", "()Ljava/lang/String;", "onlyInInheritedClass", "onlyInSameClass", "sandbox-base"}
)
public class Visibility {
   @NotNull
   private final String onlyInSameModule = "같은 모듈에서만 사용 가능";

   @NotNull
   public final String getOnlyInSameModule$sandbox_base() {
      return this.onlyInSameModule;
   }

   private final String onlyInSameClass() {
      return "같은 클래스에서만 사용 가능";
   }

   @NotNull
   protected final String onlyInInheritedClass() {
      return "하위 클래스에서만 사용 가능";
   }
}
// VisibilityKt.java
package dev.hodol.sandbox.base;

import kotlin.Metadata;
import org.jetbrains.annotations.NotNull;

@Metadata(
   mv = {1, 4, 3},
   bv = {1, 0, 3},
   k = 2,
   d1 = {"\u0000\n\n\u0000\n\u0002\u0010\u000e\n\u0002\b\u0004\"\u000e\u0010\u0000\u001a\u00020\u0001X\u0086T¢\u0006\u0002\n\u0000\"\u000e\u0010\u0002\u001a\u00020\u0001X\u0086T¢\u0006\u0002\n\u0000\"\u000e\u0010\u0003\u001a\u00020\u0001X\u0082T¢\u0006\u0002\n\u0000\"\u000e\u0010\u0004\u001a\u00020\u0001X\u0080T¢\u0006\u0002\n\u0000¨\u0006\u0005"},
   d2 = {"ALSO_EVERYWHERE", "", "EVERYWHERE", "ONLY_IN_SAME_FILE", "ONLY_IN_SAME_MODULE", "sandbox-base"}
)
public final class VisibilityKt {
   @NotNull
   public static final String EVERYWHERE = "모든 파일에서 사용 가능";
   @NotNull
   public static final String ALSO_EVERYWHERE = "Kotlin의 기본 접근제어자는 public";
   private static final String ONLY_IN_SAME_FILE = "같은 파일에서만 사용 가능";
   @NotNull
   public static final String ONLY_IN_SAME_MODULE = "같은 모듈에서만 사용 가능";
}
```

* `class` 안에서의 `internal` 멤버의 경우 getter 메소드 이름을 변경하는데, `internal`은 Java파일에서 `public` 가시성으로 변경되므로 실수로 다른 파일에서 사용하지 않도록 모듈과 패키지의 경로를 붙인다.
