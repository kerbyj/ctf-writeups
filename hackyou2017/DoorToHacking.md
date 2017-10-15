# Reverse 100

**Door to Hacking**

**Author: Omar Ganiev (beched)**

**1. Качаем архив.**

Внутри находится **DoorToHacking.class**

Ну класс, ищем средство для декомпиляции, сразу находим JD-GUI

**2. Декомпиляция.**

Открываем этот файл через JD-GUI и получаем нормальный код(ну на сколько это возможно в случае с явой).

```Java
package rev100;

import java.io.PrintStream;
import java.math.BigInteger;
import java.nio.charset.Charset;
import java.util.ArrayList;

class DoorToHacking
{
  public static void main(String[] paramArrayOfString)
  {
    ArrayList localArrayList = new ArrayList();
    localArrayList.add("254642a5");
    localArrayList.add("4a017fcc");
    localArrayList.add("0e333713");
    localArrayList.add("74f88921");
    String str1 = "";
    String str2 = (String)localArrayList.get(3);
    for (int i = 7; i >= 1; i -= 2) {
      str1 = str1 + (char)Integer.parseInt(str2.substring(i - 1, i + 1), 16);
    }
    str1 = String.format("%08x", new Object[] { new BigInteger(1, str1.getBytes(Charset.forName("US-ASCII"))) });
    localArrayList.set(3, str1);
    str1 = "";
    str2 = (String)localArrayList.get(1);
    for (i = 7; i >= 0; i--) {
      str1 = str1 + str2.charAt(i);
    }
    localArrayList.set(1, str1);
    String str3 = "";
    str3 = str3 + (String)localArrayList.get(2);
    str3 = str3 + (String)localArrayList.get(1);
    str3 = str3 + (String)localArrayList.get(3);
    str3 = str3 + (String)localArrayList.get(0);
    if ((paramArrayOfString.length >= 1) && (paramArrayOfString[0].equals(str3))) {
      System.out.println("Welcome!");
    } else {
      System.out.println("Go away!");
    }
  }
}
```

**3. Преобразуем программу для выдачи флага**

Я использовал https://codepad.remoteinterview.io

```Java
import java.io.PrintStream;
import java.math.BigInteger;
import java.nio.charset.Charset;
import java.util.ArrayList;

class DoorToHacking
{  
  public static void main(String[] paramArrayOfString)
  {
    ArrayList localArrayList = new ArrayList();
    localArrayList.add("254642a5");
    localArrayList.add("4a017fcc");
    localArrayList.add("0e333713");
    localArrayList.add("74f88921");
    String str1 = "";
    String str2 = (String)localArrayList.get(3);
    for (int i = 7; i >= 1; i -= 2) {
      str1 = str1 + (char)Integer.parseInt(str2.substring(i - 1, i + 1), 16);
    }
    str1 = String.format("%08x", new Object[] { new BigInteger(1, str1.getBytes(Charset.forName("US-ASCII"))) });
    localArrayList.set(3, str1);
    str1 = "";
    str2 = (String)localArrayList.get(1);
    for (int i = 7; i >= 0; i--) {
      str1 = str1 + str2.charAt(i);
    }
    localArrayList.set(1, str1);
    String str3 = "";
    str3 = str3 + (String)localArrayList.get(2);
    str3 = str3 + (String)localArrayList.get(1);
    str3 = str3 + (String)localArrayList.get(3);
    str3 = str3 + (String)localArrayList.get(0);
    
    System.out.println(str3);
    }
} 
```

Нажимаем Run и забираем флаг - **0e333713ccf710a4213f3f74254642a5**
