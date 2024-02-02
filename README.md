# Ruby için JSON uygulaması

[![CI](https://github.com/flori/json/actions/workflows/ci.yml/badge.svg)](https://github.com/flori/json/actions/workflows/ci. yml)

## Tanım

Bu, RFC 7159'a göre JSON spesifikasyonunun bir uygulamasıdır
http://www.ietf.org/rfc/rfc7159.txt . Orada 1.0.0 sürümünden başlayarak
iki varyant mevcut olacak:

* Iconv ve stringscan'e dayanan saf bir Ruby çeşitidir.
   her ikisi de Ruby standart kütüphanesinin parçası olan uzantılardır.
* Kısmen daha hızlı olan yerel uzantı çeşidi
   C veya Java'da uygulanır ve kendi unicode dönüşümüyle birlikte gelir
   işlevleri [Ragel] durum makinesi derleyicisi tarafından oluşturulan bir ayrıştırıcıdır.

JSON oluşturucunun her iki çeşidi de varsayılan olarak UTF-8 karakter dizilerini şu şekilde oluşturur:
Eğer gerçek değere sahip bir :ascii\_only seçeneği verilirse, görünmez
ASCII olmayan ve \uXXXX çıkış dizilerine sahip kontrol karakterleri ve desteği
Tüm aralığı oluşturabilmek için UTF-16 vekil çiftleri
Unicode kod noktaları kullanılmalıdır.

JSON dizeleri olarak kodlanacak tüm dizeler UTF-8 bayt olmalıdır
Ruby tarafındaki diziler eğer UTF-8 olmayan ham ikili dizeleri kodlamak için
kodlanmışsa, lütfen String'in to\_json\_raw\_object yöntemini kullanın (bu,
bayt dizisi içeren bir nesne) ve alıcıdaki sonucun kodunu çözecektir.


## Kurulum

JSON'un uzantı versiyonunun kullanılması tavsiye edilir çünkü JSON'dan daha hızlıdır
Ruby. Sisteminize kuramazsanız, yerleşebilirsiniz.

Komut satırına root olarak yazmanız yeterli:

''''
# rake install
''''

Yukarıdaki komut uzantıları oluşturacak ve sisteminize yükleyecektir.

''''
# rake install_pure
''''

veya

''''
# ruby install.rb
''''

sadece JSON'un pure Ruby uygulamasını kuracağım.

Rubygems kullanıyorsanız yazabilirsiniz

''''
# gem install json
''''

bunun yerine en yeni JSON sürümünü yüklemek için.

Ayrıca ruby'nin yüklenebilen yalnızca saf Ruby json çeşidi de vardır.

''''
# gem kurulumu json_pure
''''

## Uzantıları kendiniz derleyin

Eğer `parser.c` dosyasını `parser.rl` dosyasından oluşturmak ya da güzel çizim yapmak istiyorsanız
Durumlarını görüntülemek grafik görüntüleri ile görüntülemek için [Ragel]'e ihtiyacınız var.

## Kullanım

JSON'u kullanmak için şunları yapabilirsiniz

ruby
'json' gerektirir
''''

kuruluma değişkeni yüklemek için (ya `'json'` uzantısı ya da pure
değişken `'json_pure'`). Yazarak uzantı varyantını veya saf varyantı seçin.

Uzantı varyantını yüklediyseniz şunları yapabilirsiniz:

Ruby
'json/ext' gerektirir
''''

veya

Ruby
'json/pure' gerektirir
''''

Artık bir JSON belgesini arayarak Ruby veri yapısına ayrıştırabilirsiniz.

yakut
JSON.parse(belge)
''''

Ruby veri yapısı çağrısından bir JSON belgesi oluşturmak istiyorsanız
Ruby
JSON.generate(data)
''''

Ayrıca `pretty_generate` yöntemini de kullanabilirsiniz (bu, ayrıntılı ve güzel bir şekilde biçimlendirir)
`fast_generate` (bu, güvenlik önlemlerinin hiçbirini yapmaz)


Ayrıca bir String'de ayrıştırmayı kullanan JSON ve JSON[] yöntemleri de vardır veya
bir diziden veya karmaşık bir yapıdan JSON belgesi oluşturun:

Ruby
belge = JSON 'test' => 23 # => "{\"test\":23}"
belge = JSON['test' => 23] # => "{\"test\":23}"
''''

Ve

Ruby
veri = JSON '{"test":23}' # => {"test"=>23}
veri = JSON['{"test":23}'] # => {"test"=>23}
''''

Aşağıdaki durumlarda Ruby Core'un nesnelerine bir takım ortak eklemeler yüklemeyi seçebilirsiniz:


Ruby
'json/add/core' gerektirir
''''

Bunu talep ettikten sonra şunları yapabilirsiniz,
örn. örneğin, Ruby aralıklarını serileştirme/seri durumdan çıkarma:

Ruby
JSON JSON(1..10) # => 1..10
''''

JSON desteğini diğer sınıflarınıza veya kendi sınıflarınıza nasıl ekleyeceğinizi öğrenmek için
aşağıdaki "Diğer Örnekler" bölümüne bakın.

Rails'in JSON uygulamasıyla en iyi uyumluluğu elde etmek için şunları yapabilirsiniz:

Ruby
'json/add/rails' gerektirir
''''

Her iki ekleme de, eğer varsa, ilk önce `'json'` (yukarıdaki gibi) gerektirmeye çalışır.


## İstisnaların serileştirilmesi

JSON modülü varsayılan olarak "İstisna"yı genişletmez. Bir 'İstisna'yı dönüştürürseniz
JSON'a itiraz ederseniz, varsayılan olarak yalnızca istisna mesajını içerecektir.

Tüm ayrıntıları eklemek için belirtilen 'json/add/core' dosyasını yüklemelisiniz
veya özel olarak istisna eklemesini yükleyin aşağıdaki gibi:

Ruby
'json/add/istisna' gerektirir
''''

## Daha fazla örnek

Ruby veri yapısından bir JSON belgesi oluşturmak için şunu arayabilirsiniz:
'JSON.generate' şöyle:

Ruby
json = JSON.generate [1, 2, {"a"=>3.141}, false, true, nil, 4..10]
# => "[1,2,{\"a\":3.141},false,true,null,\"4..10\"]"
''''

Bir JSON belgesinden Ruby veri yapısını geri almak için aramanız gerekir.
Üzerinde JSON.parse:

Ruby
JSON.parse json
# => [1, 2, {"a"=>3.141}, yanlış, doğru, sıfır, "4..10"]
''''

Orijinal veri yapısından aralığın basit olduğunu unutmayın.
Bunun nedeni JSON'un aralıkları desteklememesidir.
veya keyfi sınıflandırmalarıdır. Bu durumda json kütüphanesi çağrıya geri döner.
'Object#to_json', '#to_s.to_json' ile aynıdır.

JSON desteği serileştirmesini isteğe bağlı sınıflara eklemek mümkündür.
