# Controller

![Untitled](Untitled.png)

### ****Action Results (Dönüş Tipleri)****

- ActionResult →  Bir sayfaya karşılık olarak gelen View yapısı, Action Result dönüş tipidir.
- ContentResult → String ifade döndürmek istediğimizde kullanabileceğimiz dönüş tipidir.
- JsonResult → AJAX isteklerinde kullanacağımız dönüş tiplerinden bir tanesidir.
- EmptyResult

![Untitled](Untitled%201.png)

```csharp
//OrnekController.cs
using Microsoft.AspNetCore.Mvc;

namespace MyAspNetCoreApp.Web.Controllers
{
    public class OrnekController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }

        public IActionResult ContentResult()
        {
            return Content("ContentResult String");
        }

        public IActionResult JsonResult()
        {
            return Json(new { Id = 1, name = "kalem 1", price = 100 });
        }

        public IActionResult EmptyResult()
        {
            return new EmptyResult();
        }

    }
}
```

```csharp

//Index.cshtml
@{
    ViewData["Title"] = "Index";
}

<h1>Ornek Controller, Index Sayfası</h1>
```

![Untitled](Untitled%202.png)

![Untitled](Untitled%203.png)

![Untitled](Untitled%204.png)

![Untitled](Untitled%205.png)

<aside>

💡 return View(); ile View dönüldüğünde .cshtml sayfası oluşturmamız gerekir. Diğer action methodlar için .cshtml sayfası oluşturmamıza gerek yoktur.

</aside>

### RedirectoAction Method

Bir action methottan başka bir action methoda yönlendirme yapabilmemizi sağlar.

Kullanım Alanı: 

Kullanıcıdan form üzerinden bilgileri alıp işledikten sonra kullanıcıyı farklı bir sayfaya yönlendirmek istersek bu methodu kullanabiliriz.  Kullanıcyı yönlendirdiğimiz sayfada ise “Bilgileriniz alınmıştır” vb. şeyler yazarak kullanıcıya bilgi verebiliriz.

```csharp
//OrnekController.cs
public IActionResult Index()
{
    return View();
}

public IActionResult Index2()
{
    return RedirectToAction("Index", "Ornek");
}
// Index -> Gidilecek sayfa
// Ornek -> Controller
```

![Untitled](Untitled%206.png)

- localhost:7188/ornek/index

![Untitled](Untitled%207.png)

- localhost:7188/ornek/index2

```csharp
app.MapControllerRoute(
name: "default",
pattern: "{controller=Home}/{action=Index}/{id?}");
```

<aside>

💡 default action Index olduğu için localhost:7188/ornek ile localhost:7188/ornek/index aynı şeyi temsil eder.

</aside>