# Method Chaining

Rust programlarımızda kodumuzu hem imperative stilde hem de fonksiyonel stilde yazabiliriz. Fonksiyonel stil, genellikle kodumuzu daha kısa ve okunabilir tutmamızı sağlar.

Imperative Stil ve Fonksiyonel Stili kullanarak aynı işlemi yapacağımız bir fonksiyon yazalım.

Imperative Stil: 

    fn main() {
        let mut rakamlar = Vec::new(); // Rakamlar isminde yeni bir vektör oluşturduk. Vektörü mutable oluşturma sebebimiz vektör içerisindeki elemanlarda değişiklik yapacak olmamız.
        let mut rakam = 0; // Rakam adında mutable bir değişken oluşturduk. Rakam değişkenimizi mutable tanımlama sebebimiz rakam değerini while döngümüz içerisinde değiştirecek olmamız.

        while rakam <= 9 {  // 0'dan ( rakamın mevcut değeri ) 9'a kadar tüm rakamları vektörümüze eklemek için while döngüsü
            rakamlar.push(rakam); // Mevcut rakam değerini vektörümüze ekliyoruz. 
            rakam += 1;
        }
        println!("{:?}", rakamlar);
    }

    Kodumuz çalıştırıldığında bize rakamlar vektörümüzü yazdıracaktır.

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

Fonksiyonel Stil:

    fn main() {
        let rakamlar = (0..=9).collect::<Vec<i32>>(); // 0 'dan 9 'a kadar olan tüm rakamları - içerisinde i32 barındıracak olan bir vektör içerisine .collect() metodu ile topluyoruz.

        //Bu şekilde de yazılabilir :
        //let rakamlar: Vec<i32> = (0..=9).collect();   

        println!("{:?}", rakamlar);
    }

** .collect() metodunu kullanarak farklı veri tipleri için koleksiyonlar oluşturabiliriz.

Fonksiyonel stili kullanarak Method Chaining işlemini gerçekleştirebiliriz.

Method Chaining - Yöntem/Metod Zincirleme

    fn main(){
        let rakamlar = vec![0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
        let vec = rakamlar.into_iter().skip(2).take(3).collect::<Vec<i32>>();
        println!("{:?}", vec);
    }

    Bu kod bize [2, 3, 4] çıktısını verecektir.

Method chaining kullanarak işlemleri tek bir satırda yapabilmemiz yanında, 
Aynı kodu daha okunabilir hale getirmek için satırlara yayabiliriz :

    fn main(){
        let rakamlar = vec![0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

        let vec = rakamlar
            .into_iter() // rakamlar dizimizi iterable hale getirdik. - Bu sayede her bir eleman üzerinde işlem uygulayabiliriz.
            .skip(2) // 2 eleman atla
            .take(3) // 3 eleman al 
            .collect::<Vec<i32>>(); // Bu elemanları, eleman olarak i32 barındıracak bir Vektörün içine topla.

        println!("{:?}", vec);
    }

Fonksiyonel Stili ve Metod Zincirleme işlemlerini, Iteratör ve Closure yapılarını öğrendikten sonra daha verimli kullanabiliriz.

# Iterator

Iteratör, bir koleksiyon içerisindeki elemanları her seferinde birer tane olmak üzere döndüren bir yapıdır.

Iteratör yapısını kullanmak istediğimizde karşımıza 3 farklı kullanım yöntemi çıkar:

.iter(): Nesnelerin referanslarını kullanan Iteratör. 
<br><br/>
.into_iter(): Nesnelerin kendisini kullanan Iteratör.
<br><br/>
.iter_mut(): Nesnelerin değiştirilebilir (mutable) referanslarını kullanan Iteratör.
<br><br/>

Iteratör yapısını örnek olarak şu şekilde kullanabiliriz:

    fn main() {
        
        // Bir vektör oluşturuyoruz
        let vector = vec![1, 2, 3];
        
        // vector üzerinde `.iter()` kullanarak elemanların referansları alınarak bir Iteratör oluşturuluyor.
        let vector_a = vector
            .iter() // Iteratör oluşturuldu
            .map(|x| x + 1) // Iteratör yapımızdan dönen her elemana (burada x ismini verdik) 1 ekliyoruz.
            .collect::<Vec<i32>>(); // Yeni değerleri bir vektör içerisine topla.

        // vector üzerinde `.into_iter()` kullanarak elemanların referansları değil, kendileri alınıyor.
        let vector_b = vector
            .into_iter() // Değerleri al ve vector'ü tüket, bu aşamadan sonra "vector" vektörümüze ulaşamayacağız.
            .map(|x| x * 10) // Iteratör yapımızdan dönen her elemanı 10 ile çarp.
            .collect::<Vec<i32>>(); // Yeni değerleri bir vektör içerisine topla.

        // Mutable bir vektör oluşturuyoruz
        let mut vector_c = vec![10, 20, 30];

        // vector_c üzerinde `.iter_mut()` kullanarak elemanlara değiştirilebilir referanslar alınıyor
        vector_c.iter_mut().for_each(|x| *x += 100); // Her elemanı 100 artır

        // Sonuçları yazdır
        println!("{:?}", vector_a);
        println!("{:?}", vector_b);
        println!("{:?}", vector_c);
    }

    Kodumuz bize çıktı olarak bu çıktıyı verecektir.

        [2, 3, 4]
        [10, 20, 30]
        [110, 120, 130]

Iteratör oluşturma işlemimizin ardından kullandığımız ilk metod olan .map() etkileşime girdiği objenin tüm elementlerine parantez içerisinde yazdığımız işlemi uygulamamızı sağlar.
Ardından nesnelerin değiştirilmiş hallerini döndürür.

Kullandığımız bir diğer metod olan .for_each()
Tıpkı map metodu gibi etkileşime girdiği objenin tüm elementlerine parantez içerisine yazdığımız işlemi uygular.
Lakin .for_each() .map()'ten farklı olarak değiştirilen bu elemanları döndürmez.
.iter_mut() ve ardından for_each() kullanımı, basitçe for döngüsü kullanmak olarak algılanabilir.

Kullandığımız her metodun içerisinde, işlem yapmak istediğimiz tüm nesnelere bir isim verebiliriz.
Burada x adını verdik.
Ardından isim verdiğimiz bu x'i değişiklik yapmak için kullanabiliriz.
Kullandığımız bu yapı Closure olarak adlandırılır.

# Closure
Closure'lar, hızlıca tanımlanabilen ve isme ihtiyaç duymayan fonksiyonlardır. Rust'ta Closure'lar genellikle || sembolü ile tanımlanır. 
Closure kullanımı Rust üzerinde oldukça yaygındır.

Bir değişkene Closure atayabiliriz. Bu sayede bu değişkeni tıpkı bir fonksiyon gibi kullanabiliriz.

    fn main() {
        let greet = || println!("Merhaba Dünya!");
        greet();
    }

    Bu kullanımda Closure herhangi bir parametre almaz(||) ve 'Merhaba Dünya!'. mesajını yazdırır.

Closure'lar parametre alabilirler:

    fn main() {
        let add = |x: i32, y: i32| x + y;
        println!("Toplam: {}", add(3, 5));
    }

Closure yapılarımız daha karmaşık haline geldiğinde, kod bloğu şeklinde yazmaya devam edebiliriz. 

    fn main() {
        let print_result = || {
            let result = 11 * 11;
            println!("Sonuç: {}", result);
        };
        print_result();
    }

Closure'lar dışarıdan değişken alabilirler:

    fn main() {
        let external = 10; 
        let multiply_externals = || external * external;
        println!("Çarpım: {}", multiply_externals());
    }

Genellikle Closure'ları bir metodun içerisinde kullanılırken görürüz. 

    fn main() {
        let vec = vec![1, 2, 3];
        let result = vec.get(3).unwrap_or_else( 
            // vec vektörünün 3 indexte elemanı var mı ? get metodumuz, varsa 'Some(3.indexteki değer)' yoksa 'None' döndürecektir. 
            // Bu sebeple unwrap metodumuzu kullanıyoruz.  Değer var ise unwrap Some(x) içerisinden x'i çıkartma işlemini uygulayacak. 
            // Değerin olmadığı durumları da değerlendirmek istediğimiz için burada sadece unwrap yerine unwrap_or_else metodumuzu kullandık. 
            // Değer gelmediğinde yapılacak işlemleri Closure içerisinde belirliyoruz.
            ||  {
                    if vec.get(0).is_some() { // Vektörün içerisine bak ve index 0 da bir şey var mı yok mu kontrol et.
                        &vec[0]; // Eğer varsa bana vektörümün 0. indexindeki elemanı döndür.
                    }
                    else{
                            &0 // Yoksa 0 döndür.
                    }
                }
        );
        println!("{}", result);
    }

Closure kullanımı ile Metod Chaining kullanarak kodumuzu daha okunabilir ve kısa hale getirebiliriz.





