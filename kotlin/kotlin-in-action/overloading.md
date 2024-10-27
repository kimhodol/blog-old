# 함수/메소드 Overloading



```kotlin

package dev.hodol.sandbox.base

const val DEFAULT_DELIMITER = ","

@JvmOverloads
fun <T> joinToString(
    collection: Collection<T>,
    separator: String? = DEFAULT_DELIMITER,
    prefix: String? = "",
    postfix: String? = ""
): String {
    TODO()
}

```

```java
package dev.hodol.sandbox.base;

import java.util.Collection;
import kotlin.Metadata;
import kotlin.NotImplementedError;
import kotlin.jvm.JvmOverloads;
import kotlin.jvm.internal.DefaultConstructorMarker;
import kotlin.jvm.internal.Intrinsics;
import org.jetbrains.annotations.NotNull;
import org.jetbrains.annotations.Nullable;

@Metadata(
   mv = {1, 4, 3},
   bv = {1, 0, 3},
   k = 2,
   d1 = {"\u0000\u0012\n\u0000\n\u0002\u0010\u000e\n\u0002\b\u0003\n\u0002\u0010\u001e\n\u0002\b\u0004\u001a@\u0010\u0002\u001a\u00020\u0001\"\u0004\b\u0000\u0010\u00032\f\u0010\u0004\u001a\b\u0012\u0004\u0012\u0002H\u00030\u00052\n\b\u0002\u0010\u0006\u001a\u0004\u0018\u00010\u00012\n\b\u0002\u0010\u0007\u001a\u0004\u0018\u00010\u00012\n\b\u0002\u0010\b\u001a\u0004\u0018\u00010\u0001H\u0007\"\u000e\u0010\u0000\u001a\u00020\u0001X\u0086T¢\u0006\u0002\n\u0000¨\u0006\t"},
   d2 = {"DEFAULT_DELIMITER", "", "joinToString", "T", "collection", "", "separator", "prefix", "postfix", "sandbox.sandbox-base.main"}
)
public final class StringFunctionsKt {
   @NotNull
   public static final String DEFAULT_DELIMITER = ",";

   @JvmOverloads
   @NotNull
   public static final String joinToString(@NotNull Collection collection, @Nullable String separator, @Nullable String prefix, @Nullable String postfix) {
      Intrinsics.checkNotNullParameter(collection, "collection");
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

   @JvmOverloads
   @NotNull
   public static final String joinToString(@NotNull Collection collection, @Nullable String separator, @Nullable String prefix) {
      return joinToString$default(collection, separator, prefix, (String)null, 8, (Object)null);
   }

   @JvmOverloads
   @NotNull
   public static final String joinToString(@NotNull Collection collection, @Nullable String separator) {
      return joinToString$default(collection, separator, (String)null, (String)null, 12, (Object)null);
   }

   @JvmOverloads
   @NotNull
   public static final String joinToString(@NotNull Collection collection) {
      return joinToString$default(collection, (String)null, (String)null, (String)null, 14, (Object)null);
   }
}

```

* Java에서는 Method Overloading을 자주 볼 수 있는데, Kotlin에서 Default Parameters를 넣어도 기본적으로는 Overloading 식으로의 지원은 해주지 않는다. 이를 위해서 @JvmOverloads를 함수에 붙여주면 여러 형태의 함수/메소드를 사용할 수 있다.
