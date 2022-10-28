# Entity Framework

![Untitled](Untitled.png)

# EF Core Nedir ?

![Untitled](Untitled%201.png)

### ORM

- Veriye erişim teknolojisi
- Nesne map’leme (Veritabanındaki ilgili tablolara karşılık kod tarafında bir class ile karşılamak - Tablodan her data cekmek istediğimizde o tablodaki datalara karşılık bir class’ın nesne örneğinin üretilmesidir)
- Veri tabanına erişim sağlarken eski usller kullanmak yerine ORM aracı kullanmak hızımızı arttırır. EF Core dışındaki veri erişim teknolojilerinde de kullanılır.

### LINQ

- EF Core’da sorgularımızı yazmak için kullandığımız teknolojidir.
- Sorgulama tekniğidir.
- Dile entegre sorgulama (Kullandığımız C# dilinin özelliklerini kullanarak çeşitli veri kaynaklarına karşı sorgulamalar yazabiliyoruz)
- C# ‘ın başlarında LINQ sorgularının kullanıldığı yerler…
    1. LINQ To Objects Collection → Memory’de tutulan Collection’lara karşılık LINQ sorguları yazabiliyoruz. (List, LinkedList, Dictionary Sınıfları, Array, Memoryde Tutulan Objeler)
        - LINQ tip güvenli bir şekilde sorgular yazmamızı sağlar. (Program çalıştırılmadan derleme sırasında derleyici yanlış yazımları uyarır)
    2. LINQ to XML 
    3. LINQ to Entities 

EF Core ile sadece tip güvenli bir sorgular yazmak zorunda değiliz. Ham SQL cümlecikleri de yazabiliriz. 

---

### Entities

- Bir class ‘dır.
- Diğer class ‘lardan farklı olarak veri tabanında bir tablo karşılığı vardır. ( Örneğin, Product class’ına karşılık veri tabanında Product tablosu vardır)

---

# ORM Nedir ?

ORM nesne mapleme tekniğidir. 

EF Core gibi Hibernate gibi veriye erişimi soyutlayan ve nesneye map ‘leyen kütüphaneler ORM aracı olarak adlandırılır.

![Untitled](Untitled%202.png)

context → Veritabanı

User → Veritabanındaki tablo

![Untitled](Untitled%203.png)

Ef Core, User tablosu üzerinde listeleme yaptığımızda yani bu tablodan data almak istediğimizde; tablodaki her bir satıra karşılık User Class ‘ından nesne örneği üretir.

User class ‘ındaki her bir property User tablosunda bir sutuna karşılık gelir.

1. id Id property’sine, name Name property’sine email Email property’sine maplenir. 
2. Her satırdan User nesne örneği oluşturur. 
3. Daha sonra tüm User ‘ları istediğimizde bu User ‘lardan liste oluşturulur.

---

![Untitled](Untitled%204.png)

<aside>

📌 Tip güvenli çalışmak projenin sürdürülebilirliğini arttırır. Hatalar henüz derleme aşamasında farkedilir.

</aside>

![Untitled](Untitled%205.png)

---

# ****EF Core Yaklaşımları****

![Untitled](Untitled%206.png)

![Untitled](Untitled%207.png)

<aside>

📌 Best practice “Code First” dür.

</aside>

# Code First

![Untitled](Untitled%208.png)

![Untitled](Untitled%209.png)

![Untitled](Untitled%2010.png)

Veritabanına karşılık gelecek olan AppDbContext sınıfını oluşturalım.

![Untitled](Untitled%2011.png)

AppDbContext ‘e karşılık gelen veritabanı,

Products Entity’sine karşılık gelen Products tablosunu aşağıdaki kodlar ile oluşturalım.

![Untitled](Untitled%2012.png)

![Untitled](Untitled%2013.png)

![Untitled](Untitled%2014.png)

![Untitled](Untitled%2015.png)

### Product tablosuna data kaydetme ve tablodan data silme:

```csharp
using Microsoft.AspNetCore.Mvc;
using MyAspNetCoreApp.Web.Models;

namespace MyAspNetCoreApp.Web.Controllers
{
    public class ProductsController : Controller
    {
        private AppDbContext _context;

        private readonly ProductRepository _productRepository;

        public ProductsController(AppDbContext context)
        {
            //DI Container
            //Dependency Injection Pattern
            _productRepository = new ProductRepository();
            _context = context;

            //Linq method
            if (!context.Products.Any())
            {
                _context.Products.Add(new Product() { Name = "Kalem 1", Price = 100, Stock = 100 });
                _context.Products.Add(new Product() { Name = "Kalem 2", Price = 200, Stock = 300 });
                _context.Products.Add(new Product() { Name = "Kalem 3", Price = 300, Stock = 600 });

                _context.SaveChanges();
            }
        }

        public IActionResult Index()
        {
            var products = _context.Products.ToList();   

            return View(products);
        }

        public IActionResult Remove(int id)
        {
            var product = _context.Products.Find(id);

            _context.Products.Remove(product);

            _context.SaveChanges();

            return RedirectToAction("Index");

        }

        public IActionResult Add()
        {
            return View();
        }

        public IActionResult Update(int id)
        {
            return View();
        }
    }
}
```