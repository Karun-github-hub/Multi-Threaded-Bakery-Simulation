İsim:
170421049_pasta.c - Pasta hazırlama yazılımı için toptancı ve ustalar arasında malzeme transferini sağlayan program

Kullanılan Teknolojiler:
- C Programlama Dili
- POSIX Threads (pthread) kütüphanesi
- POSIX Semaforlar (semaphore) kütüphanesi
- Mutex (Kritik bölge kontrolü için)

Kurulum:
Bu programı çalıştırmak için gerekli adımlar:
1. Kod dosyasını bir `C` derleyicisi ile derleyin.
2. Çalıştırılabilir dosyayı çalıştırın ve malzeme dosyasının yolunu belirtin.

Program Komutları:
gcc -o 170421049_pasta 170421049_pasta.c -lpthread
./170421049_pasta -i malzemeler.txt

       -i <malzemeler.txt>
              Malzeme listesini içeren dosyanın yolu.
Kullanım:
Program, toptancının malzemeleri ustalara dağıtmasını ve ustaların bu malzemeleri kullanarak pasta yapmasını simüle eder. 
Programın kullanımı şu şekildedir:
1)Programı çalıştırın:
./170421049_pasta -i malzemeler.txt
2)Program çalışırken, toptancının ve ustaların durumları hakkında bilgi veren mesajlar ekrana yazdırılacaktır.

Katkıda Bulunma:
Bu projeye katkıda bulunmak için, GitHub üzerinde projeyi forklayarak kendi depo'nuzda değişiklikler yapabilirsiniz. Değişikliklerinizi yeni bir dal (branch) oluşturarak gerçekleştirin ve commit edin. Daha sonra, dalınızı (branch) orijinal depo'ya gönderip bir Pull Request oluşturarak katkılarınızı sunabilirsiniz. Her türlü geri bildirim, hata düzeltmesi ve yeni özellik önerisi memnuniyetle karşılanacaktır. Katkılarınız için şimdiden teşekkür ederim.

Açıklama:
Program, POSIX thread (pthread) kütüphanesini kullanarak çoklu iş parçacıklarını yönetir. İş parçacıkları, toptancı ve her bir usta için ayrı ayrı oluşturulur. Bu iş parçacıkları, eşzamanlı olarak çalışabilir ve paylaşılan kaynaklara (malzemeler dizisi) erişmek için mutex ve semaforlar kullanır.
Semaforlar, toptancı ve ustalar arasında haberleşmeyi sağlar. toptanci_sem semaforu, toptancının malzemeleri teslim ettiğini ve ustaların çalışmaya başlayabileceğini bildirir. usta_sem semaforları ise her bir ustaya malzeme geldiğini haber verir.
Mutex, toptancı ve ustalar arasında paylaşılan kaynaklara (malzemeler dizisi) erişim kontrolünü sağlar. Mutex kilitlendiğinde, sadece bir iş parçacığının bu kaynaklara erişmesine izin verilir, böylece veri bütünlüğü korunur.
Toptancı işlevi, malzemeler.txt dosyasından malzemeleri okur ve malzemeleri toptancı ve ustalar arasında dağıtır. Usta işlevi ise, toptancıdan gelen malzemeleri alır, pastayı hazırlar ve toptancıya teslim eder.
Main işlevi, programın başlangıç noktasıdır. Bu işlev, komut satırı argümanlarını kontrol eder, semaforları ve mutex'i başlatır, toptancı ve ustaların iş parçacıklarını başlatır ve iş parçacıklarının tamamlanmasını bekler.
Ana iş akışı, toptancının malzemeleri göndermesi ve ustaların bu malzemeleri alıp pastaları hazırlamasıdır. Toptancı, dosyadan malzemeleri okur ve ustalara iletmek üzere semafor sinyalleri gönderir. Ustalar, gerekli malzemelerin kendilerine uygun olup olmadığını kontrol ederler. Eğer uygunsa malzemeleri alır, pastayı hazırlar ve toptancıya teslim ederler.
Program, toptancının tüm malzemeleri gönderdikten sonra done bayrağını 1 yapar ve ustalara bu durumu belirtmek için semafor sinyalleri gönderir. Böylece ustalar da işlerini bitirir ve program sonlanır.
Programın sonunda, semaforlar ve mutex serbest bırakılır.

-Valgrind ile Bellek Kullanımı Analizi
Valgrind raporuna göre, program bellek yönetimi açısından temiz olduğu ve herhangi bir bellek sızıntısının bulunmadığı görülmektedir. Özellikle, raporun belirttiği "All heap blocks were freed -- no leaks are possible" ifadesi, tüm dinamik bellek tahsislerinin program sonlandırıldığında serbest bırakıldığını ve bellek sızıntısı olmadığını doğrular.
valgrind --leak-check=full --track-origins=yes ./170421049_pasta -i malzemeler.txt
==354== 
==354== HEAP SUMMARY:
==354==     in use at exit: 0 bytes in 0 blocks
==354==   total heap usage: 16 allocs, 16 frees, 7,520 bytes allocated
==354== 
==354== All heap blocks were freed -- no leaks are possible
==354== 
==354== For lists of detected and suppressed errors, rerun with: -s
==354== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

-Ps ile Zombie Süreçlerin Tespiti
ps aux | grep 'Z'
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
runner       516  0.0  0.0   6620  2176 pts/0    S+   11:47   0:00 grep --color=auto Z
Bu sonuç, sistemde herhangi bir zombie süreç bulunmadığını göstermektedir. grep --color=auto Z komutu yalnızca grep komutunun kendisini listeler ve herhangi bir zombie sürecin olmadığını doğrular.

Programın hem bellek yönetimi hem de süreç yönetimi açısından sağlıklı olduğu ve herhangi bir sorun bulunmadığı anlaşılmaktadır. 

-Top ile Sistem Performansını İzleme
top - 11:55:11 up  9:05,  0 users,  load average: 14.38, 12.50, 10.06
Tasks:   7 total,   1 running,   6 sleeping,   0 stopped,   0 zombie
%Cpu(s): 41.5 us, 20.8 sy,  0.0 ni, 36.1 id,  0.8 wa,  0.0 hi,  0.6 si,  0.2 st
MiB Mem :  64292.4 total,  23646.1 free,  33379.4 used,   7266.9 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.  30216.6 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND      
      1 runner    20   0 2516336  45304  24192 S   0.7   0.1   0:06.14 pid1         
     16 runner    20   0   11.1g 226636  46208 S   0.7   0.3   0:06.87 node         
    713 runner    20   0    9092   3584   3072 R   0.7   0.0   0:00.02 top          
     28 runner    20   0    8292   4736   3712 S   0.0   0.0   0:00.10 bash         
     53 runner    20   0    3804   2688   2560 S   0.0   0.0   0:00.00 nix-editor   
     69 runner    20   0  840436  25000   9472 S   0.0   0.0   0:00.08 taplo        
     70 runner    20   0 1146428  81504  61324 S   0.0   0.1   0:00.90 .ccl

Bellek Yönetimi: Herhangi bir bellek sızıntısı veya aşırı bellek kullanımı yok.
CPU Kullanımı: Yüksek işlem yükü var, ancak zombie süreç yok.
Süreç Yönetimi: Tüm süreçler düzgün çalışıyor ve zombie süreç bulunmuyor.

Bu program 170421049 nolu öğrenci Karun Acar tarafından yazılmıştır.
