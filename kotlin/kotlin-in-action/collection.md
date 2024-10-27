# Collection 생성

```kotlin
package dev.hodol.sandbox.base

class Collections {
    private val set = setOf(1, 7, 53)
    private val mutableSet = mutableSetOf(1, 7, 53)
    private val hashSet = hashSetOf(1, 7, 53)

    private val list = listOf(1, 7, 53)
    private val mutableList = mutableListOf(1, 7, 53)

    private val map = mapOf(1 to "one", 7 to "seven", 53 to "fifty three")
    private val mutableMap = mutableMapOf(1 to "one", 7 to "seven", 53 to "fifty three")
    private val hashMap = hashMapOf(1 to "one", 7 to "seven", 53 to "fifty three")
}
```

```java
package dev.hodol.sandbox.base;

import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;
import kotlin.Metadata;
import kotlin.Pair;
import kotlin.TuplesKt;
import kotlin.collections.CollectionsKt;
import kotlin.collections.MapsKt;
import kotlin.collections.SetsKt;

@Metadata(
   mv = {1, 4, 3},
   bv = {1, 0, 3},
   k = 1,
   d1 = {"\u0000L\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0002\b\u0002\n\u0002\u0018\u0002\n\u0002\u0010\b\n\u0002\u0010\u000e\n\u0002\u0018\u0002\n\u0000\n\u0002\u0018\u0002\n\u0002\u0018\u0002\n\u0000\n\u0002\u0010 \n\u0000\n\u0002\u0010$\n\u0000\n\u0002\u0010!\n\u0000\n\u0002\u0010%\n\u0000\n\u0002\u0010#\n\u0000\n\u0002\u0010\"\n\u0000\u0018\u00002\u00020\u0001B\u0005¢\u0006\u0002\u0010\u0002R*\u0010\u0003\u001a\u001e\u0012\u0004\u0012\u00020\u0005\u0012\u0004\u0012\u00020\u00060\u0004j\u000e\u0012\u0004\u0012\u00020\u0005\u0012\u0004\u0012\u00020\u0006`\u0007X\u0082\u0004¢\u0006\u0002\n\u0000R\u001e\u0010\b\u001a\u0012\u0012\u0004\u0012\u00020\u00050\tj\b\u0012\u0004\u0012\u00020\u0005`\nX\u0082\u0004¢\u0006\u0002\n\u0000R\u0014\u0010\u000b\u001a\b\u0012\u0004\u0012\u00020\u00050\fX\u0082\u0004¢\u0006\u0002\n\u0000R\u001a\u0010\r\u001a\u000e\u0012\u0004\u0012\u00020\u0005\u0012\u0004\u0012\u00020\u00060\u000eX\u0082\u0004¢\u0006\u0002\n\u0000R\u0014\u0010\u000f\u001a\b\u0012\u0004\u0012\u00020\u00050\u0010X\u0082\u0004¢\u0006\u0002\n\u0000R\u001a\u0010\u0011\u001a\u000e\u0012\u0004\u0012\u00020\u0005\u0012\u0004\u0012\u00020\u00060\u0012X\u0082\u0004¢\u0006\u0002\n\u0000R\u0014\u0010\u0013\u001a\b\u0012\u0004\u0012\u00020\u00050\u0014X\u0082\u0004¢\u0006\u0002\n\u0000R\u0014\u0010\u0015\u001a\b\u0012\u0004\u0012\u00020\u00050\u0016X\u0082\u0004¢\u0006\u0002\n\u0000¨\u0006\u0017"},
   d2 = {"Ldev/hodol/sandbox/base/Collections;", "", "()V", "hashMap", "Ljava/util/HashMap;", "", "", "Lkotlin/collections/HashMap;", "hashSet", "Ljava/util/HashSet;", "Lkotlin/collections/HashSet;", "list", "", "map", "", "mutableList", "", "mutableMap", "", "mutableSet", "", "set", "", "sandbox.sandbox-base.main"}
)
public final class Collections {
   private final Set set = SetsKt.setOf(new Integer[]{1, 7, 53});
   private final Set mutableSet = SetsKt.mutableSetOf(new Integer[]{1, 7, 53});
   private final HashSet hashSet = SetsKt.hashSetOf(new Integer[]{1, 7, 53});
   private final List list = CollectionsKt.listOf(new Integer[]{1, 7, 53});
   private final List mutableList = CollectionsKt.mutableListOf(new Integer[]{1, 7, 53});
   private final Map map = MapsKt.mapOf(new Pair[]{TuplesKt.to(1, "one"), TuplesKt.to(7, "seven"), TuplesKt.to(53, "fifty three")});
   private final Map mutableMap = MapsKt.mutableMapOf(new Pair[]{TuplesKt.to(1, "one"), TuplesKt.to(7, "seven"), TuplesKt.to(53, "fifty three")});
   private final HashMap hashMap = MapsKt.hashMapOf(new Pair[]{TuplesKt.to(1, "one"), TuplesKt.to(7, "seven"), TuplesKt.to(53, "fifty three")});
}

```

* Java에서보다 조금 더 쉽게 Collection을 만들 수 있다.
* `Map`의 경우 `to` 함수(놀랍게도)가 눈에 띄는데, key-value로 이루어진 `Pair`객체를 만들고, `Map`이 `Pair`들로 구성된 형태로 보인다.
* Kotlin에서 기본적으로 값을 바꿀 수 없는 불변콜렉션을 제공한다.
  * `List`, `Set`, `Map`은 값을 추가하거나 제거할 수 없다.
  * `Mutable~` 들은 추가 혹은 제거가 가능하며, `HashMap`, `HashSet`의 경우는 `Mutable~`의 구현체다.
