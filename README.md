### 1-Docker image ile container arasındaki fark nedir ?

Docker Container, bir servisin veya yazılımın herhangi bir ortam üzerinde çalışır durumda olması için gereken bütün kod,araç sistem binaryleri ve ayarlarını barındıran izole bir yapıdır. Bunun yanında Docker image ise bu containerların kurulması için container içinde barınan şablonlar ve talimatlar setidir.

* * *

### 2-Docker ps komutu ne işe yarar.

Docker ps komutu, çalışan containerların görüntülenmesini sağlar. -a argumenti ekleyerek de oluşturulmuş olan tüm containerları gösterir.

![img1.png](/img1.png)Örnekteki cihazda 2 adet container çalıştığını görebiliyoruz. -a argumentini eklediğimizde çalışan containerlar ile beraber çalışmayan containerları da görüntüleyebiliyoruz.

* * *

### **3-Docker run -d nginx komutu ne yapar**

Bu komutu çalıştırdığımızda Docker localde nginx image arar ve nginx container'i oluşturur. nginx image'ı localde mevcut değilse docker hub üzerinden belirtilen image'ı indirmeye çalışır. -d argumenti ise containeri terminalden detach ederek arkaplanda containeri çalıştırır![img2.png](/img2.png)Cihazda nginx image'ı bulunmadığından library/nginx bölümünden nginx image'ini cihaza indirip ondan sonra containeri arkaplanda çalıştırdık.

* * *

### 4-Container silindiğinde içindeki veriler neden kaybolur ?

Docker containerlar yapısı gereği bağlı olduğu image'in dosya sisteminin üzerine eklenen yazılabilir bir katman kullanır. Container silinirse bu katman da silinmiş olur, bu sebeple veriler de kaybolur. Bu verilerin kaybolmaması isteniyorsa volume veya bind mount kullanılıp host makineye bir klasör atanabilir.

* * *

### 5-docker pull ile docker build arasındaki fark nedir ?

docker pull komutu ile Docker hub'a önceden hazırlanmış imageleri kendi sistemimize indirebiliriz.

docker build komutu ise Dockerfile ile configleri ve container oluşturulurken girilecek komutlar ile docker image oluşurmak için kullanılır.

* * *

### 6-Dockerfile'daki FROM komutu ne anlama gelir?

Dockerfile'da FROM komutu ile containera yüklenecek olan image, OS ve kullanılacak versiyon belirtilir. FROM komutu Dockerfile içerisinde birden fazla yazılabilir, birden fazla image yüklenebilir.

Örnek;

FROM nginx  || FROM nginx:alpine || FROM nginx:1.25.5-alpine

* * *

### 7-COPY ve ADD arasındaki fark nedir

Dockerfile'da kullanılan COPY ve ADD komutları kapsamları farklı olsa da benzer işlev görürler

ADD ve COPY komutları build edilecek klasörde bulunan dosyaları containerde belirli dosya yoluna kopyalar.

ADD komutu ile URL'si girilen dosyaların da aktarımı sağlanabilir. ayrıca klasörde sıkıştırılmış formatta dosyalar varsa bunları da containerde ayıklama işlemi yapar ancak COPY'de böyle bir özellik yoktur.

* * *

### 8-EXPOSE 8080 ne yapar ?

Dockerfile'a EXPOSE komutu ile port belirttiğimizde container o porttan webdeki ağ trafiğini TCP üzerinden dinlemeye başlar, UDP bağlantıları için UDP özellikle belirtilmelidir. EXPOSE komutu direkt olarak portu belirtemez, bunun yüzünden container içindeki servisin hangi porttan çalıştığını bilmek gereklidir

* * *

### 9-Dockerfile ile build edilen image hangi komutla çalıştırılır.

&nbsp;                   ` docker build -t image1 .`

komutunu kullanarak Dockerfile ile build ettiğimiz image'yi

`docker run image1`

komutu ile çalıştırabiliriz.

* * *

### 10-Volume ile bind mount arasındaki fark nedir ?

Volume Dockerfiles içerisinde VOLUME komutu ile oluşturulan kalıcı depolama alanlarıdır ve /var/lib/docker/volumes dosya yolu altında oluşur. Öbür taraftan bind mount komutu ise docker container çalıştırılırken -v argumenti ile host içerisinde belirli bir klasör belirlenerek kullanılır ve veriler oraya kaydedilir.

* * *

### 11-Container silinse bile verilerin korunması nasıl sağlanır ?

Containerdaki verilerin silinmemesi için hosttaki bir klasör belirlenmesi gerekir. Bu sayede container silinse bile volume atanan klasörde veriler kaydolmaya devam eder.

`docker run -v /opt/datadir:/var/lib/mysql mysql # Left side is where the volume will be kept in the host, right side is the docker container volume location`

Solda bulunan dosya yolu hostta verilerin kaydedileceği klasör iken sağ tarafta bulunan dosya yolu docker containerindeki verilerin içerdiği bölümdür.

* * *

### 12-Docker network tipleri nelerdir ?

Docker network tipleri bridge, host, none, overlay, macvlan ve ipvlan den oluşmaktadır.

bridge: Containerler için varsayılan network tipidir. containerlar bu tip ile birbirleriyle haberleşebilir ve host vasıtasıyla internete NAT ile çıkabilirler.

host: Bu tipte container hostun ağını kullanır ve aradaki izolasyon kaldırılır.

none: Ağı devre dışı bırakır ve host ile container arasındaki bağlantıyı tamamen izole eder.

overlay: Farklı host makinelerde çalışan docker containerlerini aynı ağda bağlanır.

macvlan: Bir containere MAC adresi atar ve containeri ağ üzerinde fiziksel cihaz olarak gösterir.

ipvlan: IPv4 ve IPv6 üzerinde tam kontrol sağlar, bu ayarla bir containere host ip üzerinden değil de containere atanan ip üzerinden erişilebilir.

* * *

### 13-Multi stage build neden kullanılır ?

Build ve Runtime environmentlarını ayırmak için Multi stage build kullanılır. Multi stage buildde FROM komutları aşama aşama işlenir ve final aşamaya gelindiğinde kopyalanmamış bütün dosyalar son aşamada silinir. Bu sayede Container'da ihtiyaç olmayan dosyalar bulunmaz bu sayede daha temiz ve yönetimi daha kolay bir docker containeri çalıştırılmış olur.Golang dosyayı derledikten sonra derleyici containerdan çıkarıldığında boyut büyük oranda küçülüyor.

* * *

### 14-Docker hub'a image pushlamak için hangi komut kullanılır ?

Docker hub'a image pushlamak için öncelikle kullanıcı girişi yapılmalıdır.

`docker login -u username`

Ardından containerimize bir tag vermemiz gerekir

`docker tag image1 username/image1:latest`

Tag verdikten sonra docker push komutuyla Docker huba pushlayabiliriz

`docker push username/image1:latest`

* * *

### 15-Docker system prune ne yapar ?

docker system prune komutu hostta bulunan çalışmayan bütün containerları, imageları, ağları ve build cachelerini temizlemek için kullanılır.

`docker system prune -a # tagları olmayanlar dahil bütün imageları kaldırır`

`docker system prune --filter # belirli koşullara göre silme işlemi yapar`

`docker system prune --volumes # containerlar için oluşturulmuş volumeları da beraberinde siler.`

* * *

### 16-Container logları nasıl görüntülenir ?

Container loglarını görüntülemek için `docker container logs CONTAINER_NAME` veya `docker logs CONTAINER_NAME` Kullanılabilir.

`docker logs --follow CONTAINER_NAME # --follow argumenti ile loglar sürekli görüntülenebilir`

`docker logs --details CONTAINER_NAME # configde belirtildiyse environment variablelar dahil logları görüntüler`

* * *

### 17-Production için lightweight image kullanmanın avantajları nelerdir ?

Lightweight imagelerin sadece uygulamayı çalıştırmak için gerekli olan dosyaları içermesi sisteme daha az yük bindirmeleri ve dosya boyutu küçüklüğü açısından avantaj sağlamaktadır.

![img3.png](/img3.png)

Örnekte görüldüğü gibi nginx in debian sürümü 192MB disk alanı kaplarken aynı uygulamanın alpine sürümü 52.5 MB yer kaplamaktadır. Lightweight image kullanarak büyük oranda disk boyutunda avantaj sağlanabilmektedir.

* * *

### 18-Docker Flagları

**docker run**

- \-p port belirtilirken kullanılır "docker run -p 8080:80"
- \-d containeri arkaplanda çalıştırır "docker run -d"
- \-e environment variable "docker run -e "APP_COLOR=BLUE"
- \--name oluşturulacak containere isim verir "docker run --name CONTAINER_NAME"
- \-v containera bind mount eklenir "docker run -v /docker/containers/CONTAINER_NAME"
- \-it terminal ile aktif çalıştırılıp container içinde komut çalıştırılabilir "docker run -it"

**docker build**

- \-t oluşturulacak image'a isim verilir "docker build -t IMAGE_NAME"
- \--target Multi stage build yaparken hangi stage'in son stage olduğu belirtilir. "docker build --target builder -t IMAGE_NAME ."

**docker system prune**

- \-a Kullanılmayan tüm kaynakları siler "docker system prune -a"
- \--filter Hangi kaynakların silineceği belirtilir "docker system prune --filter"

**docker logs**

- \--follow belirlenen containerdaki logları canlı takip eder "docker logs --follow CONTAINER_NAME"
- \--tail  belirlenen sayıdaki son satırı görüntüler "docker logs --tail 50 CONTAINER_NAME"

**docker exec**

- \-it container içinde interaktif terminal açılmasını sağlar "docker exec -it"

&nbsp;

* * *

### 19-Dockerfile komutları nelerdir

**FROM**

From komutu hangi image'nin indirileceğini belirtmek için kullanılır. İlk komut FROM ile başlar ve bir Dockerfile'da birden fazla FROM komutu olabilir.

FROM nginx:alpine

**RUN**

Run komutu terminal üzerinde hangi komutun gönderileceğini belirtir ve komutun işlemi bitirmesini bekler. O esnada containerda hangi işlemin yürütüleceğini belirtir.

RUN echo hello world

RUN pip install flask

**ADD**

Containera dosya eklenmesi gerekiyorsa bu komut kullanılır. Dosyaların nereden ve nereye kopyalanacağı belirtilir ve hosttan containera doğru kopyalama yapılabilir.

ADD index.html /usr/share/nginx/html/index.html

Klasördeki index.html dosyasını alıp containerda belirtilen dosya yoluna yerleştirir.

**ENV**

ENV komutu containera hem build hem de runtime aşamasında environment variable belirtmek için kullanılır.

ENV APP_COLOR=blue

**ENTRYPOINT**

Bu komut container her başladığında terminale komut ne ayarlandıysa onu çalıştırır ve override edilemez.

ENTRYPOINT \["ping"\]

docker run CONTAINER_NAME google.com # komutu girildiğinde

&nbsp;container "ping google.com" şeklinde komut çalıştırır.

**CMD**

CMD bir uygulama başlatılması veya script açılması gerekiyorsa cmd üzerinden komut çalıştırılarak bu işlevi yerine getirir, kolaylıkla override edilebilir.

CMD \["echo", "Hello World"\]

docker run CONTAINER_NAME

çalıştırdığımızda container echo Hello World komutunu terminalde çalıştırır.

* * *

### 20-bir tane multistage pipeline ile multistage olmayan pipeline arasındaki farkları hem file olarak hem diğer farklılıklar olarak bize gösterebilir misin?

Örnekte golang dilinde yazılmış olan kod dosyası ilk golangda derleniyor ardından ikinci aşamaya geçtiğinde kullanılmayan go dosyaları containerden çıkartılıyor. Bu sayede son aşamaya kopyalanmayan tüm dosyalar ve build araçları silinir ve image dosyası boyut açısından daha az yer kaplamış olur. 

```
#STAGE 1
FROM golang:1.22 AS build WORKDIR /app 
COPY . . 
RUN go build -o main . 

#STAGE 2
FROM alpine:3.20 
WORKDIR /app 
COPY --from=build /app/main . 
CMD \["./main"\]
```

```
$ docker images
REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
redis           latest    f1285ef3611d   4 weeks ago    137MB
nginx           latest    41f689c20910   4 weeks ago    192MB
golang-alpine   latest    411bc66a7adb   2 months ago   102MB # Multi-stage
golang          latest    2fce09cfad57   7 months ago   823MB # Non Multi-stage
```

Örnekte görüldüğü gibi iki farklı image aynı dosyayı derleyip çalıştırıyor. birinin kapladığı dosya boyutu diğerinden çok daha az oluyor. Multi-stage image build ederek image'ların daha kolay yönetilebilmesini ve oluşturulan image'ların daha güvenli bir yapıda olmasını sağlayabiliyoruz.
