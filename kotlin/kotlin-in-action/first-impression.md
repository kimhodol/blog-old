# Kotlin의 첫 인상

#### `Person.kt`

```kotlin
package dev.hodol.sandbox.base

data class Person (
    val name: String,
    val age: Int? = null,
)

fun main(args: Array<String>) {
    val people = listOf(Person("호돌"), Person("운장", 28))
    val oldest = persons.maxByOrNull { it.age ?: 0 }
    println("나이가 가장 많은 사람: $oldest")
    // 나이가 가장 많은 사람: Person(name=운장, age=28)
}
```

`build`시에 다음 파일들로 변경된다.&#x20;

#### `build/classes/.../Person.class`

```java
package dev.hodol.sandbox.base

public final data class Person public constructor(name: kotlin.String, age: kotlin.Int? /* = compiled code */) {
    public final val age: kotlin.Int? /* compiled code */

    public final val name: kotlin.String /* compiled code */

    public final operator fun component1(): kotlin.String { /* compiled code */ }

    public final operator fun component2(): kotlin.Int? { /* compiled code */ }
}
```

#### `build/classes/.../PersonKt.class`

```java
// IntelliJ API Decompiler stub source generated from a class file
// Implementation of methods is not available

package dev.hodol.sandbox.base

public fun main(args: kotlin.Array<kotlin.String>): kotlin.Unit { /* compiled code */ }
```

#### `Person.decompiled.java`(IntelliJ Actions - Kotlin ByteCode 메뉴 실행)

```java
// PersonKt.java
package dev.hodol.sandbox.base;

import java.util.Iterator;
import java.util.List;
import kotlin.Metadata;
import kotlin.collections.CollectionsKt;
import kotlin.jvm.internal.DefaultConstructorMarker;
import kotlin.jvm.internal.Intrinsics;
import org.jetbrains.annotations.NotNull;

@Metadata(
   mv = {1, 4, 3},
   bv = {1, 0, 3},
   k = 2,
   d1 = {"\u0000\u0014\n\u0000\n\u0002\u0010\u0002\n\u0000\n\u0002\u0010\u0011\n\u0002\u0010\u000e\n\u0002\b\u0002\u001a\u0019\u0010\u0000\u001a\u00020\u00012\f\u0010\u0002\u001a\b\u0012\u0004\u0012\u00020\u00040\u0003¢\u0006\u0002\u0010\u0005¨\u0006\u0006"},
   d2 = {"main", "", "args", "", "", "([Ljava/lang/String;)V", "sandbox-base"}
)
public final class PersonKt {
   public static final void main(@NotNull String[] args) {
      Intrinsics.checkNotNullParameter(args, "args");
      List people = CollectionsKt.listOf(new Person[]{new Person("호돌", (Integer)null, 2, (DefaultConstructorMarker)null), new Person("운장", 28)});
      Iterable $this$maxByOrNull$iv = (Iterable)people;
      int $i$f$maxByOrNull = false;
      Iterator iterator$iv = $this$maxByOrNull$iv.iterator();
      Object var10000;
      if (!iterator$iv.hasNext()) {
         var10000 = null;
      } else {
         Object maxElem$iv = iterator$iv.next();
         if (!iterator$iv.hasNext()) {
            var10000 = maxElem$iv;
         } else {
            Person it = (Person)maxElem$iv;
            int var8 = false;
            Integer var15 = it.getAge();
            int maxValue$iv = var15 != null ? var15 : 0;

            do {
               Object e$iv = iterator$iv.next();
               Person it = (Person)e$iv;
               int var10 = false;
               var15 = it.getAge();
               int v$iv = var15 != null ? var15 : 0;
               if (maxValue$iv < v$iv) {
                  maxElem$iv = e$iv;
                  maxValue$iv = v$iv;
               }
            } while(iterator$iv.hasNext());

            var10000 = maxElem$iv;
         }
      }

      Person oldest = (Person)var10000;
      String var11 = "나이가 가장 많은 사람: " + oldest;
      $i$f$maxByOrNull = false;
      System.out.println(var11);
   }
}
// Person.java
package dev.hodol.sandbox.base;

import kotlin.Metadata;
import kotlin.jvm.internal.DefaultConstructorMarker;
import kotlin.jvm.internal.Intrinsics;
import org.jetbrains.annotations.NotNull;
import org.jetbrains.annotations.Nullable;

@Metadata(
   mv = {1, 4, 3},
   bv = {1, 0, 3},
   k = 1,
   d1 = {"\u0000 \n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0000\n\u0002\u0010\u000e\n\u0000\n\u0002\u0010\b\n\u0002\b\u000b\n\u0002\u0010\u000b\n\u0002\b\u0004\b\u0086\b\u0018\u00002\u00020\u0001B\u0019\u0012\u0006\u0010\u0002\u001a\u00020\u0003\u0012\n\b\u0002\u0010\u0004\u001a\u0004\u0018\u00010\u0005¢\u0006\u0002\u0010\u0006J\t\u0010\f\u001a\u00020\u0003HÆ\u0003J\u0010\u0010\r\u001a\u0004\u0018\u00010\u0005HÆ\u0003¢\u0006\u0002\u0010\bJ$\u0010\u000e\u001a\u00020\u00002\b\b\u0002\u0010\u0002\u001a\u00020\u00032\n\b\u0002\u0010\u0004\u001a\u0004\u0018\u00010\u0005HÆ\u0001¢\u0006\u0002\u0010\u000fJ\u0013\u0010\u0010\u001a\u00020\u00112\b\u0010\u0012\u001a\u0004\u0018\u00010\u0001HÖ\u0003J\t\u0010\u0013\u001a\u00020\u0005HÖ\u0001J\t\u0010\u0014\u001a\u00020\u0003HÖ\u0001R\u0015\u0010\u0004\u001a\u0004\u0018\u00010\u0005¢\u0006\n\n\u0002\u0010\t\u001a\u0004\b\u0007\u0010\bR\u0011\u0010\u0002\u001a\u00020\u0003¢\u0006\b\n\u0000\u001a\u0004\b\n\u0010\u000b¨\u0006\u0015"},
   d2 = {"Ldev/hodol/sandbox/base/Person;", "", "name", "", "age", "", "(Ljava/lang/String;Ljava/lang/Integer;)V", "getAge", "()Ljava/lang/Integer;", "Ljava/lang/Integer;", "getName", "()Ljava/lang/String;", "component1", "component2", "copy", "(Ljava/lang/String;Ljava/lang/Integer;)Ldev/hodol/sandbox/base/Person;", "equals", "", "other", "hashCode", "toString", "sandbox-base"}
)
public final class Person {
   @NotNull
   private final String name;
   @Nullable
   private final Integer age;

   @NotNull
   public final String getName() {
      return this.name;
   }

   @Nullable
   public final Integer getAge() {
      return this.age;
   }

   public Person(@NotNull String name, @Nullable Integer age) {
      Intrinsics.checkNotNullParameter(name, "name");
      super();
      this.name = name;
      this.age = age;
   }

   // $FF: synthetic method
   public Person(String var1, Integer var2, int var3, DefaultConstructorMarker var4) {
      if ((var3 & 2) != 0) {
         var2 = (Integer)null;
      }

      this(var1, var2);
   }

   @NotNull
   public final String component1() {
      return this.name;
   }

   @Nullable
   public final Integer component2() {
      return this.age;
   }

   @NotNull
   public final Person copy(@NotNull String name, @Nullable Integer age) {
      Intrinsics.checkNotNullParameter(name, "name");
      return new Person(name, age);
   }

   // $FF: synthetic method
   public static Person copy$default(Person var0, String var1, Integer var2, int var3, Object var4) {
      if ((var3 & 1) != 0) {
         var1 = var0.name;
      }

      if ((var3 & 2) != 0) {
         var2 = var0.age;
      }

      return var0.copy(var1, var2);
   }

   @NotNull
   public String toString() {
      return "Person(name=" + this.name + ", age=" + this.age + ")";
   }

   public int hashCode() {
      String var10000 = this.name;
      int var1 = (var10000 != null ? var10000.hashCode() : 0) * 31;
      Integer var10001 = this.age;
      return var1 + (var10001 != null ? var10001.hashCode() : 0);
   }

   public boolean equals(@Nullable Object var1) {
      if (this != var1) {
         if (var1 instanceof Person) {
            Person var2 = (Person)var1;
            if (Intrinsics.areEqual(this.name, var2.name) && Intrinsics.areEqual(this.age, var2.age)) {
               return true;
            }
         }

         return false;
      } else {
         return true;
      }
   }
}

```
