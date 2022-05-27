# Java 8 ile gelen yenilikler
## Functional Interfaces
Lambda ifadelerini uygulayabilmek için getirilen arayüzler ile javanın functional programming yetenekleri geliştirilmeye başlandı.
## Lambda expressions
Herhangi bir sınıfa ait olmadan iş yapabilen pratik fonksiyonlar olan Lambda ifadeler ile birlikte Java, funtional programming dünyasına da girmiş oldu.
## Method References
Metodları, nesneler veya primitive değerlermiş gibi kullanabilmemizi ve bunları başka bir metoda parametre olarak gönderebilmemizi sağlıyor.
:: söz dizimi aracılığıyla static methodlar class name ile, static olmayan methodlar ise instance objeleri ile referans verilebilmekte.
## Stream API
Collection’lar üzerinde filter, forEach, map, count, min/max gibi sık kullanılan çeşitli işlemleri yapmayı çok daha kolay hale getiriyor.
## Try-With Resources
File, connection, network vb. açık kaynakları kapatmaya gerek duymadan, AutoClosable interface ile dene-kullan-kapat şeklinde otomatik olarak kapanacak bir kullanım imkan sağlar.
## Optional Class
Nesnelerin kullanılmadan önce yapılan NullCheck işlemlerini daha okunabilir ve kontrol edilebilir olmasını sağlayan Optional<T> utility sınıfını sağlıyor.
## Date and Time API
Tarih ve saat verilerinin daha doğal, net ve anlaşılması kolay şekilde elde edilmesini sağlıyor.
## Concurrency Improvements
Thread nesneleri oluşturmak ve yönetmek zorunda kalma eşzamanlı concurrent/multitasking işlemlerini daha anlaşılır ve kolay kullanılabilir hale getiriyor.

# Java 11 ile gelen yenilikler
## Launch Single-File Source-Code Programs – JEP 330
Java dosyasınını hızlıca derlemeden çalıştırma özelliği gelmiştir.
```bash
java MerhabaJava.java
```
## Local-Variable Syntax for Lambda Parameters – JEP 323
Java 10 ile gelen var ifadesinin kapsamı genişletilerek lambda expressions ile kullanma imkanı gelmiştir.
```bash
public class App {

    public static void main(String[] args) {
        IntStream.of(1, 2, 3, 5, 6, 7)
                .filter((var i) -> i % 3 == 0)
                .forEach(System.out::println);
    }

}
```
Ayrıca final anahtar kelimesi ile birlikte kullanılabilmektedir.



## Nest-Based Access Control – JEP 181
Sınıf içerisinde yer alan alt sınıfların derlenmesinde değişiklik yapılarak nest olarak adlandırılan bir yapı kurulmuştur.
```bash
public class App {

    public static void main(String[] args) {
        Alt.yazdir();
    }

    static class Alt {
        private static void yazdir() {
            System.out.println("Yusuf SEZER");
        }
    }

}
```
Yukarıdaki kodlar derlenip javap aracı ile incelendiğinde NestMembers, NestHost alanları görünecektir.


## HTTP Client (Standard) – JEP 321
Java 9 ile birlikte gelen HTTP/2 Client Java 11 ile standart olarak java.net.http paketinde gelmektedir.
```bash
public class App {

    public static void main(String[] args)
            throws IOException, InterruptedException {

        HttpClient httpClient = HttpClient.newHttpClient();
        HttpRequest httpRequest = HttpRequest
                .newBuilder()
                .uri(URI.create("https://www.yusufsezer.com/"))
                .GET()
                .build();

        HttpResponse response = httpClient
                .send(httpRequest, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());

    }

}
```
Standart paketi kullanmak için java.net.http; modülünün module-info.java dosyasına eklenmesi gerekir.
## Remove the Java EE and CORBA Modules – JEP 320
Java 9 ile deprecated olarak işaretlenen ve Java SE paketi içerisinde yer alan Java EE ile CORBA modülleri kaldırılmıştır.

Kaldırılan paketler aşağıda yer almaktadır.

java.xml.ws
java.xml.bind
java.activation
java.xml.ws.annotation
java.corba
java.transaction
java.se.ee
jdk.xml.ws
jdk.xml.bind
Java 11 ile birlikte XML işlemleri için JAXB paketinin JAR veya Maven olarak bağımlılık olarak eklenmesi gerekmektedir.
## String Methods
String işlemlerini kolaylaştırmak için yeni metotlar eklenmiştir.

strip, stripLeading, stripTrailing – trim metodundan farkı Unicode desteğinin olmasıdır.
```bash

public class App {

    public static void main(String[] args) {
        var myName = "   Yusuf Sefa SEZER  ";
        System.out.println(myName.strip().length());
        var myName2 = "   Yusuf Sefa SEZER  ";
        System.out.println(myName2.stripLeading().length());
        System.out.println(myName2.stripTrailing().length());
    }

}
```
isBlank – String ifadesinin boşluk, Unicode boşluk değerine göre kontrol eder.
```bash

public class App {

    public static void main(String[] args) {
        var myName = " \u205F ";
        System.out.println(myName.isBlank());  // içeriğe bakar
        System.out.println(myName.isEmpty());  // uzunluğa bakar
    }

}
```
lines – String ifadesindeki satır sonlandırıcına göre Stream API değeri verir.
```bash
public class App {

    public static void main(String[] args) {
        var myName = "Yusuf\nSefa\nSEZER\r\n\ry";
        System.out.println(myName.lines().count());
    }

}
```
repeat – String ifadesini parametre değeri kadar tekrar eder.
```bash
public class App {

    public static void main(String[] args) {
        var myName = "Yusuf Sefa SEZER\n";
        System.out.println(myName.repeat(3));
    }

}
```
## Collection
Koleksiyonları dizilere çevirmek için toArray(IntFunction) metodu eklenmiştir.
```bash
public class App {

    public static void main(String[] args) {
        String[] myNames = List
                .of("Yusuf", "Sefa", "SEZER")
                .toArray(String[]::new);
        System.out.println(myNames.length);
    }

}
```
## Files
Dosya okuma ve yazma işlemlerini hızlıca yapmak için readString, writeString metotları eklenmiştir.
```bash
public class App {

    public static void main(String[] args) throws IOException {
        // writeString
        Path tempFile = Files.writeString(
                Files.createTempFile("test", ".txt"), "Yusuf Sefa SEZER");
        System.out.println(tempFile);

        // readString
        String fileContent = Files.readString(tempFile);
        System.out.println(fileContent);
    }

}
```
## Path
Dosya/Dizin yolunu ifade etmek için kullanılan Path arayüzüne of metotları eklenmiştir.
```bash
public class App {

    public static void main(String[] args) throws IOException {
        // of(string)
        Path pathString = Path.of("C:", "temp", "test.txt");
        System.out.println(pathString);
        System.out.println(Files.exists(pathString));

        // of(uri)
        URI uri = URI.create("file:///C:/temp/test.txt");
        System.out.println(uri);
        Path pathURI = Path.of(uri);
        System.out.println(pathURI);
        System.out.println(Files.exists(pathURI));
    }

}
```
## java.io
Java ile Input/Output işlemlerinde kullanılan java.io paketine yeni nullInputStream, nullOutputStream, nullReader, nullWriter, readNBytes metotları eklenmiştir.
```bash
public class App {

    public static void main(String[] args) throws IOException {
        // nullReader
        Reader nullReader = Reader.nullReader();
        System.out.println(nullReader.read());

        // nullWriter
        Writer nullWriter = Writer.nullWriter();
        nullWriter.write("Yusuf SEZER");
    }

}
```
```bash
public class App {

    public static void main(String[] args) throws IOException {
        var stream = new ByteArrayInputStream("Yusuf Sefa SEZER".getBytes());
        System.out.println(new String(stream.readNBytes(5)));
        stream.close();
    }

}
```
FileReader ve FilterWriter sınıflarına yeni kurucu metotlar eklenmiştir.
## java.lang.Class
Nest-based yapısına göre derlenen sınıflar ile ilgili detaylı bilgi almak için Class sınıfına getNestHost, getNestMembers, isNestmateOf metotları eklenmiştir.
```bash
public class App {

    public static void main(String[] args) {
        Class<?> clazz = App.class;
        System.out.println(clazz.getNestHost().getSimpleName());
        Arrays
                .stream(clazz.getNestMembers())
                .map(Class::getSimpleName)
                .forEach(System.out::println);
    }

    static class Alt {
        private static void yazdir() {
            System.out.println("Yusuf SEZER");
        }
    }

}
```
# Java 17 ile gelen yenilikler
## Vector API (Incubator) – JEP 338
Java JIT derleyicisine otomatik vektör özelliği geliştirmesidir. Bu özellik sayesinde Java içerisindeki skalar veya tek işlemin otomatik olarak vektöre dönüştürülmesi ve vektörel işlemlerde elde edilen(SIMD ile) performansın elde edilmesi amaçlanmaktadır. Özellik Java derleyicisinde(JIT) yapılmıştır.
## Enable C++14 Language Features – JEP 347
JDK kodlarına C++14 dili desteği eklendi. Özellik sayesinde JDK C++14 ile birlikte gelen özelliklerin kullanımı sağlanmış oldu.

C++ 14 ile gelen özelliklere C++ 14 üzerinden ulaşabilirsiniz.
## Migrate from Mercurial to Git – JEP 357
Java derleyici kaynak kodları Mercurial sürm kontrol sisteminin fazla kaynak kullanımı(dosya boyutu), Mercurial araçlarının yetersiz olması gibi nedenlerden dolayı Git sürüm sistemine geçiş yapılmıştır.
## Migrate to GitHub – JEP 369
JEP 357 ile Git sürüm sistemine geçiş ile birlikte Git tabanlı Github kullanımına geçiş yapılmıştır.


## ZGC: Concurrent Thread-Stack Processing – JEP 376
ZGC çöp toplayıcısında değişiklik yapılarak ZGC çöp toplayıcısının büyük boyutlu verilerle iyi ve kısa sürede çalışmasını sağlar.
## Unix-Domain Socket Channels – JEP 380
İşlemler arasında haberleşme için TCP/IP yerine Unix temelli haberleşme yöntemine(AF_UNIX) geçilmiştir. Özellik Unix temelli MacOS, Linux temelli işletim sistemleri ile birlikte Windows 10 tarafından da desteklenmeye başlanmasıyla kullanılabilir hale gelmiştir.
## Alpine Linux Port – JEP 386
Bulut tabanlı işletim sistemlerinde sıklıkla kullanılan Alpine işletim sistemi desteği eklenmiştir. Böylece bir çok Java kütüphanesinin Alpine ile birlikte çalışmasını sağlar.
## Elastic Metaspace – JEP 387
Java uygulamalarına ait kullanılmayan verilerin(metadata) hızlı bir şekilde işletim sistemine iade edilmesini sağlanmıştır.


## Windows/AArch64 Port – JEP 388
AArch64 veya ARM64 işlemci desteği eklenmiştir.
## Foreign Linker API (Incubator) – JEP 389
C veya C++ kodlarına doğrudan erişim imkanı getirilmiştir. Bu sayede karmaşık JNI işlemleri daha basit hale gelmiştir.
```bash
MethodHandle strlen = CLinker.getInstance().downcallHandle(
        LibraryLookup.ofDefault().lookup("strlen").get(),
        MethodType.methodType(long.class, MemoryAddress.class),
        FunctionDescriptor.of(CLinker.C_LONG_LONG, CLinker.C_POINTER)
);
```
Yukarıdaki kod parçası C dilinde yer alan strlen metoduna erişmektedir.

LibraryLookup ile erişilecek metot belirlenmiştir.

MethodType ile metodun Java’ya döndürülme biçimi belirtilmiştir.

FunctionDescriptor ile ise C dilindeki karşılığı belirtilmiştir.

NOT: İşletim sistemine göre C dili tanımı(FunctionDescriptor kullanımı) farklılık gösterebilir.

Aşağıda örnek kullanım yer almaktadır.
```bash
MethodHandle strlen = CLinker.getInstance().downcallHandle(
        LibraryLookup.ofDefault().lookup("strlen").get(),
        MethodType.methodType(long.class, MemoryAddress.class),
        FunctionDescriptor.of(CLinker.C_LONG_LONG, CLinker.C_POINTER)
);

String str = "Yusuf SEZER";
MemorySegment memorySegment = CLinker.toCString(str); // Java String adres bilgisi alınıyor
long result = (long) strlen.invokeExact(memorySegment.address()); // strlen metodu String adres bilgisi ile çalıştırılıyor
System.out.println(result);
memorySegment.close();
```
CLinker#toCString metodu yardımıyla str değişkeninin adres bilgisi alınarak MethodHandle#invokeExact metoduna parametre olarak gönderilmektedir.

MethodHandle#invokeExact adres bilgisini parametre olarak alarak C komutlarını çalıştırır.

Sonuç istenilen türe çevrilerek kullanılır.
```bash
import java.lang.invoke.MethodHandle;
import java.lang.invoke.MethodType;
import jdk.incubator.foreign.CLinker;
import jdk.incubator.foreign.FunctionDescriptor;
import jdk.incubator.foreign.LibraryLookup;
import jdk.incubator.foreign.MemoryAddress;
import jdk.incubator.foreign.MemorySegment;

public class App {

    public static void main(String[] args) throws Throwable {

        MethodHandle strlen = CLinker.getInstance().downcallHandle(
                LibraryLookup.ofDefault().lookup("strlen").get(),
                MethodType.methodType(long.class, MemoryAddress.class),
                FunctionDescriptor.of(CLinker.C_LONG_LONG, CLinker.C_POINTER)
        );

        String str = "Yusuf SEZER";
        MemorySegment memorySegment = CLinker.toCString(str);
        long result = (long) strlen.invokeExact(memorySegment.address());
        System.out.println("Uzunluk: " + result);
        memorySegment.close();
    }

}
```
Özelliğin kullanımı için jdk.incubator.foreign modülünün proje eklenmesi gerekmektedir.

Modül eklenerek komutlar derlenir.
```bash
javac --add-modules jdk.incubator.foreign App.java
```
Derlendikten sonra çalıştırılır.
```bash
java --add-modules jdk.incubator.foreign -Dforeign.restricted=permit App
```
## Warnings for Value-Based Classes – JEP 390
Değer taşıyan veya @jdk.internal.ValueBased ifadesi ile belirtilen sınıfların senkronizasyonu sırasında uyarı verecektir.

Aşağıdaki komut derlenme aşamasında uyarı verecektir.
```bash
Double d = 20.0;
synchronized (d) {}
```
Java 9 ile birlikte değer taşıyan, sarmalayıcı(wrapper) sınıfların kullanımdan kaldırılan kurucu metotları @Deprecated(since=”9″, forRemoval = true) olarak işaretlenmiştir.
## Packaging Tool – JEP 392
Java 14 sürümüyle birlikte gelen paketleme özelliğinin devamıdır.

Özellik sayesinde ek kütüphaneye ihtiyaç duymadan işletim sistemine göre(deb, rpm, pkg, dmg, msi, exe) kurulum paketi oluşturmayı sağlar.

Swing örneğinde yer alan aşağıdaki kodları kullanarak derleme paketleme işlemini yapalım.
```bash
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;

public class App {

    public static void main(String[] args) {

        JFrame pencere = new JFrame();
        pencere.setTitle("Java Swing");
        pencere.setSize(300, 200);

        JLabel label = new JLabel("www.yusufsezer.com");
        label.setBounds(50, 50, 150, 20);
        pencere.add(label);

        JButton button = new JButton("Tıkla");
        button.setBounds(50, 70, 60, 20);
        pencere.add(button);

        pencere.setLayout(null);
        pencere.setLocationRelativeTo(null);
        pencere.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        pencere.setVisible(true);

    }

}
```
Komutları derleyelim.
```bash
javac App.java
```
Derlenen komutları paketleyelim.
```bash
jar cvf app.jar App.class
```
Paketleme aracını kullanarak işletim sistemine göre kurulum paketi haline getirelim.
```bash
jpackage --input . --name App --main-jar app.jar --main-class App
```
Kurulum dosyası kullanılarak işletim sistemine göre kurulum yapılır.

Kurulum dosyası ayarları ile ilgili detaylı bilgi için araç kullanımına bakmak faydalı olacaktır.
```bash
jpackage --help
```
## Foreign-Memory Access API (Third Incubator) – JEP 393
Java 14 ve 15 sürümlerinde geliştirilmesi devam eden farklı bellek alanına erişim özelliğinin geliştirilmesinin devamıdır.
## Pattern Matching for instanceof – JEP 394
Java 14 ve 15 sürmlerinde gelen pattern matching özelliği standart hale gelerek Java’ya eklenmiştir.
## Records – JEP 395
Java 14 ve 15 sürümlerinde gelen records özelliği standart hale gelerek Java’ya eklenmiştir.
## Strongly Encapsulate JDK Internals by Default – JEP 396
Java 9 ile birlikte JDK içerisinde yer alan sınıflara erişim engellenmiş ancak kullanan sınıfların işleyişi bozulmaması için –illegal-access değeri varsayılan olarak permit değeri ile izin verilmiştir.

Java 16 ile birlikte varsayılan olarak deny olarak değiştirilerek JDK sınıflarının doğrudan kullanımı engellenmiştir.
## Sealed Classes (Second Preview) – JEP 397
Java 15 sürümü ile birlikte gelen Sealed Classes özelliğinin devamıdır.


### KAYNAKÇA
- https://medium.com/huawei-developers-tr/java-versiyonlar%C4%B1-ve-gelen-yenilikler-8-16-1d024561b5b9
- https://www.yusufsezer.com.tr/