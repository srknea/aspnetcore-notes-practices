# Model

![Untitled](Untitled.png)

![Untitled](Untitled%201.png)

![Untitled](Untitled%202.png)

![Untitled](Untitled%203.png)

![Untitled](Untitled%204.png)

```csharp
// Product.cs
namespace MyAspNetCoreApp.Web.Models
{
    public class Product
    {
        public int Id { get; set; }

        public string Name { get; set; }

        public decimal Price { get; set; }

        public int Stock { get; set; }

    }
}
```

![Untitled](Untitled%205.png)

```csharp

// ProductRepository.cs

namespace MyAspNetCoreApp.Web.Models
{
    public class ProductRepository
    {
        // liste oluşturma
        /* statik yapıldı çünkü uygulama ayağa kalkınca memoryde olsun ve
        uygulama kapanana kadar bu listeleme üzerinden çalışabilelim */
        
        private static List<Product> _products = new List<Product>();

        /* static olan bir uyeye direkt olarak class ismi üzerinden erişebiliriz nesne
        örneği almamıza gerek yoktur. Fakat şuan privite olduğu içimn şuanda her class tarafından
        erişilemeyecek bu nedenle listemizi aşağıdaki tanımladığımız methodlar ile dış dünyaya açacağız
        yani başka classlar bu repository üzerinde işlem yapabilecek*/

        /*
        private List<Product> GetAll()
        {
            return _products;
        }
        */

        public List<Product> GetAll() => _products;

        public void Add(Product newProduct) => _products.Add(newProduct);

        public void Remove(int id)
        {
            var hasProduct = _products.FirstOrDefault(x => x.Id == id);

            if(hasProduct != null)
            {
                throw new Exception($"Bu id({id}) 'ye sahip ürün bulunmamaktadır !");
            }

            _products.Remove(hasProduct);
        }

        public void Update(Product updateProduct)
        {
            var hasProduct = _products.FirstOrDefault(x => x.Id == updateProduct.Id);

            if (hasProduct != null)
            {
                throw new Exception($"Bu id({updateProduct.Id}) 'ye sahip ürün bulunmamaktadır !");
            }

            hasProduct.Name = updateProduct.Name;
            hasProduct.Price = updateProduct.Price;
            hasProduct.Stock = updateProduct.Stock;

            var index = _products.FindIndex(x => x.Id == updateProduct.Id);

            _products[index] = hasProduct;
        }

    }
}
```

![Untitled](Untitled%206.png)

![Untitled](Untitled%207.png)

![Untitled](Untitled%208.png)

![Untitled](Untitled%209.png)

![Untitled](Untitled%2010.png)

![Untitled](Untitled%2011.png)

![Untitled](Untitled%2012.png)

```csharp
// ProductsController.cs 

using Microsoft.AspNetCore.Mvc;
using MyAspNetCoreApp.Web.Models;

namespace MyAspNetCoreApp.Web.Controllers
{
    public class ProductsController : Controller
    {
        private readonly ProductRepository _productRepository;

        public ProductsController()
        {
            if (!_productRepository.GetAll().Any())
            {
                _productRepository = new ProductRepository();
                _productRepository.Add(new() { Id = 1, Name = "Kalem1", Price = 100, Stock = 200 });
                _productRepository.Add(new() { Id = 2, Name = "Kalem2", Price = 200, Stock = 400 });
                _productRepository.Add(new() { Id = 3, Name = "Kalem3", Price = 300, Stock = 600 });

            }
        }

        public IActionResult Index()
        {
            var products = _productRepository.GetAll();

            return View();
        }
    }
}
```