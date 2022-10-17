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

## RedirectoAction Method

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
//Program.cs
app.MapControllerRoute(
name: "default",
pattern: "{controller=Home}/{action=Index}/{id?}");
```

<aside>

💡 default action Index olduğu için https://localhost:7188/ornek ile https://localhost:7188/ornek/index aynı şeyi temsil eder.

</aside>

## ****Action Method Parametre Tanımlama****

Action methodlar arasında yönlendirme yapma:

```csharp
//Program.cs
app.MapControllerRoute(
name: "default",
pattern: "{controller=Home}/{action=Index}/{id?}");
```

Action methodlar arasında yönlendirme yapma:

```csharp
//OrnekController.cs
/*
Parametre isimleri OrnekController.cs dosyasındaki isimlerle aynı olmalıdır.
{id?}
*/
public IActionResult ParametreView()
{
    return RedirectToAction("JsonResultParametre", "Ornek", new { Id = 222 });
}
public IActionResult JsonResultParametre(int id)
{
    return Json(new { Id = id, name = "Kalem", price = 100 });
}
```

![Untitled](Untitled%208.png)

Yukarıdaki linki yazınca aşağıdaki sonucu alırız…

![Untitled](Untitled%209.png)

Action methodlar arasında yönlendirme yapma ve başka bir action methoda parametre geçme işlemi:

```csharp
//Program.cs
app.MapControllerRoute(
name: "default",
pattern: "{controller=Home}/{action=Index}/{id?}");
```

```csharp
//OrnekController.cs
/*
Parametre isimleri OrnekController.cs dosyasındaki isimlerle aynı olmalıdır.
{id?}
(int id)
*/
public IActionResult ParametreView(int id)
{
    return RedirectToAction("JsonResultParametre", "Ornek", new { Id = id });
}
public IActionResult JsonResultParametre(int id)
{
    return Json(new { Id = id, name = "Kalem", price = 100 });
}
```

![Untitled](Untitled%2010.png)

Yukarıdaki linki yazınca aşağıdaki sonucu alırız…

![Untitled](Untitled%2011.png)

---

# ****Veri Taşıma Yöntemleri (Controller-View)****

- ViewBag
- ViewData
- TempData
- ViewModel

## ViewBag

```csharp
//OrnekController.cs
---
public IActionResult Index()
{
    ViewBag.name = "Serkan";

    return View();
}
---
```

```csharp

//Index.cshtml

@{
    ViewData["Title"] = "Index";
}

<h1>Ornek Controller, Index Sayfası</h1>

<p>@ViewBag.name</p>

@{
    var name = ViewBag.name;
    <p>@name</p>
}

<p>@name</p>
```

![Untitled](Untitled%2012.png)

```csharp
//OrnekController.cs
---
public IActionResult Index()
{
    ViewBag.person = new { Id = 231, Name = "Serkan" };

    return View();
}
---
```

```csharp
//Index.cshtml

@{
    ViewData["Title"] = "Index";
}

<h1>Ornek Controller, Index Sayfası</h1>

<p>@ViewBag.person.Id -  @ViewBag.person.Name</p>

```

![Untitled](Untitled%2013.png)

## ****ViewData****

```csharp
//OrnekController.cs
---
public IActionResult Index()
{
		ViewData["age"] = 23;

    return View();
}
---
```

```csharp
//Index.cshtml

@{
    ViewData["Title"] = "Index";
}

<h1>Ornek Controller, Index Sayfası</h1>

<p>Yaş: @ViewData["age"]</p>
```

![Untitled](Untitled%2014.png)

```csharp
//OrnekController.cs
---
public IActionResult Index()
{
		ViewData["names"] = new List<string>() { "Serkan", "Esmanur", "Elif" };

    return View();
}
---
```

```csharp
//Index.cshtml

@{
    ViewData["Title"] = "Index";
}

<h1>Ornek Controller, Index Sayfası</h1>

<h1>İsimler</h1> 
@foreach (var item in ViewData["names"] as List<string>)
{
    <p>@item</p>
}
```

![Untitled](Untitled%2015.png)

<aside>

📌 ViewBag ve ViewData kullanıldığı action methodun kendi .cshtml sayfasına veri taşır.

</aside>

<aside>

📌 TempData, bir action methottan başka bir action methoda veri taşımak için kullanılır.

</aside>

## TempData

![Untitled](Untitled%2016.png)

Index.cshtml sayfasından TextIndex.cshtml sayfasına veri taşıyalım…

```csharp
public IActionResult Index()
{

    TempData["brand"] = "Microsoft";

    return View();
}

public IActionResult TestIndex()
{
    return View();
}
```

```csharp
//TestIndex.cshtml

@{
    ViewData["Title"] = "TestIndex";
}

<h1>TestIndex Sayfası</h1>

<p>Marka: @TempData["brand"]</p>
```

![Untitled](Untitled%2017.png)

## ViewModel

<aside>

💡 ViewModel hacimli dataları taşımak için kullanılır. Örneğin, bir tabloya veri taşımak için.

</aside>

```csharp
//OrnekController.cs
---
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public IActionResult Index()
{
    var productList = new List<Product>()
    {
       //new () {Id=1, Name="Cetvel"},
       new Product() {Id=1, Name="Cetvel"},
       new Product() { Id = 2, Name = "Çanta" },
       new Product() { Id = 3, Name = "Silgi" }
    };

    return View(productList);
}
---
```

```csharp
//Index.cshtml
@{
    ViewData["Title"] = "Index";
}

<h1>Ornek Controller, Index Sayfası</h1>

@foreach (var item in Model)
{
    <p>@item.Id - @item.Name</p>
}

//Eğitim videosundaki kodu yazınca CS0246 hatası alınıyor...
//https://learn.microsoft.com/en-us/dotnet/csharp/misc/cs0273
```

![Untitled](Untitled%2018.png)