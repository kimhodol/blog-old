# Property

```kotlin
package dev.hodol.sandbox.base

class Person (
    var name: String,
    var isMarried: Boolean
)
```

```java
package dev.hodol.sandbox.base;

import kotlin.Metadata;
import kotlin.jvm.internal.Intrinsics;
import org.jetbrains.annotations.NotNull;

@Metadata(
   mv = {1, 4, 3},
   bv = {1, 0, 3},
   k = 1,
   d1 = {"\u0000\u0018\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0000\n\u0002\u0010\u000e\n\u0000\n\u0002\u0010\u000b\n\u0002\b\t\u0018\u00002\u00020\u0001B\u0015\u0012\u0006\u0010\u0002\u001a\u00020\u0003\u0012\u0006\u0010\u0004\u001a\u00020\u0005¢\u0006\u0002\u0010\u0006R\u001a\u0010\u0004\u001a\u00020\u0005X\u0086\u000e¢\u0006\u000e\n\u0000\u001a\u0004\b\u0004\u0010\u0007\"\u0004\b\b\u0010\tR\u001a\u0010\u0002\u001a\u00020\u0003X\u0086\u000e¢\u0006\u000e\n\u0000\u001a\u0004\b\n\u0010\u000b\"\u0004\b\f\u0010\r¨\u0006\u000e"},
   d2 = {"Ldev/hodol/sandbox/base/Person;", "", "name", "", "isMarried", "", "(Ljava/lang/String;Z)V", "()Z", "setMarried", "(Z)V", "getName", "()Ljava/lang/String;", "setName", "(Ljava/lang/String;)V", "sandbox.sandbox-base.main"}
)
public final class Person {
   @NotNull
   private String name;
   private boolean isMarried;

   @NotNull
   public final String getName() {
      return this.name;
   }

   public final void setName(@NotNull String var1) {
      Intrinsics.checkNotNullParameter(var1, "<set-?>");
      this.name = var1;
   }

   public final boolean isMarried() {
      return this.isMarried;
   }

   public final void setMarried(boolean var1) {
      this.isMarried = var1;
   }

   public Person(@NotNull String name, boolean isMarried) {
      Intrinsics.checkNotNullParameter(name, "name");
      super();
      this.name = name;
      this.isMarried = isMarried;
   }
}
```

* `is`로 시작하지  않는 이상 Kotlin의 property는 Java로 변환될 때 `get`이 붙은 getter 메소드가 추가된다.
  * setter의 경우 `setIsMarried`가 아닌 `setMarried`로 변경된다.
