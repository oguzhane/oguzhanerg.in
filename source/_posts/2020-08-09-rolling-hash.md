---
layout: blog
title: Rolling Hash
date: 2016-12-28T20:32:00.000Z
---
`Recursive hashing` veya `rolling checksum` olarak da bilinen `rolling hash` input olarak verilen stringin tümünü işleme tabii tutmadan hash degerini hesaplamaya olanak tanıyan bir hash fonksiyonudur. Bunun avantajını bir örnek üzerinden açıklayalım.

- - -

Elimizde `X` ve `Y` değişkenleri olsun ve string degerleri tutsunlar. Problemimiz şu şekilde:
`X`in degeri `Y`nin degeri ile aynı mı?Karşılaştırın. Bunun için bize kesin cevabı veren maliyetli yöntemimiz şu şekilde: `Y` stringinin her bir karakterini `X` stringinin o indisteki karakteriyle karşılaştırmak. İşte bu yöntem maliyetli, çirkin bir yöntem. Olabildiğince bundan kaçınmamız gerek, bu aşamaya varmadan daha az maliyetli bir yöntemle `evet(aynı)` veya `hayır(farklı)` sonuçlarına ulaşabiliyorsak ulaşmamız gerek.
Daha iyi anlamak için X ve Y değişkenleri şunun gibi olsun.
`X="abc"`
`Y="bcd"`
değişkenlerin değerlerine bakarak; `hayır bcd stringi abc stringi ile aynı değil` diyebiliriz.

- - -

Değişkenleri basit bir hash fonksiyonuna tabii tutalım.
$H_1 = H(abc) = a*7^0 +  b*7^1 + c*7^2 = 97*1+98*7+99*49=5634$
$H_1 = 5634$
$H_2 = H(bcd) = b*7^0 +  c*7^1 + d*7^2 =98*1+99*7+100*49=5691$
$H_2 = 5691$
Üstte hash degerlerini hesapladık. Bunun için taban aritmetiğinden yararlandık. Basit olması açısından 7 tabanını göz önüne aldık. Sonra harflerin ascii degerlerini, soldan sağa doğru basamak ağırlıklarıyla çarpıp topladık.
Hash degerlerini elde ettik. $5632 \neq 5931$ olduğu için bu değerler aynı değil.

- - -

Bir süre sonra `X`in değerinin `X="bcd"` olarak değiştiğini varsayalım. Tekrar karşılaştırma yapmamız gerek. Bunun için `X`in yeni değerinin tüm karakterlerini okuyarak, son hash değerini hesaplamamıza gerek yok. Bu daha kompleks fazla sayıda karakter içeren degerler icin ciddi bir maliyet demektir. Bunun yerine daha zekice bir yöntem kullanacağız.
`X`in degerine baktığımızda baştaki `a` karakteri çıkartılmış ve sona `d` karakteri eklenmiş, O halde şu adımları uygulayalım;

* baştaki `a` karakterini kaldır. $(a*7^0+ b*7^1+c*7^2)  - a*7^0 $
* basamak ağırlıkları bir birim kaydığı için `7`ye böl. $(b*7^1+c*7^2)/7 => b*7^0 + c*7^1$
* `d` karakterinden gelecek degeri göz önüne al. $b*7^0 + c*7^1 + d*7^2$

Bu adımlar ile birlikte $H_1' = 5691 $ olacağından dolayı
$ H_1' = H_2 $ olur. Bundan sonra hash fonksiyonumuz yeterli değilse, istersek,  maliyetli yöntem ile karşılaştırmaya devam edilebilir.

#### **Kaynak**

* https://en.wikipedia.org/wiki/Rolling_hash
* https://www.quora.com/What-is-a-rolling-hash-and-when-is-it-useful