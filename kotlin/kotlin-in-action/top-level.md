# 최상위 함수와 프로퍼티

```kotlin
package dev.hodol.sandbox.base

const val DEFAULT_DELIMITER = ","

fun <T> joinToString(
    collection: Collection<T>,
    separator: String = ",",
    prefix: String = "",
    postfix: String = ""
): String {
    TODO()
}

```

```java
package dev.hodol.sandbox.base;

import java.util.Collection;
import kotlin.Metadata;
import kotlin.NotImplementedError;
import kotlin.jvm.internal.DefaultConstructorMarker;
import kotlin.jvm.internal.Intrinsics;
import org.jetbrains.annotations.NotNull;

@Metadata(
   mv = {1, 4, 3},
   bv = {1, 0, 3},
   k = 2,
   d1 = {"\u0000\u0012\n\u0000\n\u0002\u0010\u000e\n\u0002\b\u0003\n\u0002\u0010\u001e\n\u0002\b\u0004\u001a8\u0010\u0002\u001a\u00020\u0001\"\u0004\b\u0000\u0010\u00032\f\u0010\u0004\u001a\b\u0012\u0004\u0012\u0002H\u00030\u00052\b\b\u0002\u0010\u0006\u001a\u00020\u00012\b\b\u0002\u0010\u0007\u001a\u00020\u00012\b\b\u0002\u0010\b\u001a\u00020\u0001\"\u000e\u0010\u0000\u001a\u00020\u0001X\u0086T¢\u0006\u0002\n\u0000¨\u0006\t"},
   d2 = {"DEFAULT_DELIMITER", "", "joinToString", "T", "collection", "", "separator", "prefix", "postfix", "sandbox.sandbox-base.main"}
)
public final class StringFunctionsKt {
   @NotNull
   public static final String DEFAULT_DELIMITER = ",";

   @NotNull
   public static final String joinToString(@NotNull Collection collection, @NotNull String separator, @NotNull String prefix, @NotNull String postfix) {
      Intrinsics.checkNotNullParameter(collection, "collection");
      Intrinsics.checkNotNullParameter(separator, "separator");
      Intrinsics.checkNotNullParameter(prefix, "prefix");
      Intrinsics.checkNotNullParameter(postfix, "postfix");
      boolean var4 = false;
      throw (Throwable)(new NotImplementedError((String)null, 1, (DefaultConstructorMarker)null));
   }

   // $FF: synthetic method
   public static String joinToString$default(Collection var0, String var1, String var2, String var3, int var4, Object var5) {
      if ((var4 & 2) != 0) {
         var1 = ",";
      }

      if ((var4 & 4) != 0) {
         var2 = "";
      }

      if ((var4 & 8) != 0) {
         var3 = "";
      }

      return joinToString(var0, var1, var2, var3);
   }
}
```

* Java와 다르게 Kotlin에서는 `class` 내부가 아닌 최상단에 함수나 변수 선언이 가능하다.&#x20;
* 파일 이름이 `StringFunctions.kt`이므로, Java Bytecode로 변환시 `class StringFunctionsKt` 로 감싸지고, 함수나 프로퍼티는 `static`하게 생성된다. 다른 Java 파일에서 이를 사용할 때는 `StringFunctionsKt.joinToString(...)`로 사용해야한다.
* 이를 우리가 의도한 네이밍대로 사용하고 싶다면 파일 최상단(`package` 문 이전)에 `@JvmName`을 선언해주면 된다. 이를 통해 `StringFunctions.DEFAULT_DELIMITER`, `StringFunctions.joinToString(...)`로 사용할 수 있다.

```kotlin
@file:JvmName("StringFunctions")

package dev.hodol.sandbox.base

fun <T> joinToString(
    collection: Collection<T>,
    separator: String = ",",
    prefix: String = "",
    postfix: String = ""
): String {
    TODO()
}
```

