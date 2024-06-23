## انواع Action Filter
1. Authorization Filter
2. Resource Filter
3. Action Filter
4. Result Filter
5. Exception Filter

### Authorization Filter
- **Base Class:**: IAuthorizationFilter
- **Use as:** `[CustomAuthorizationFilter]`

### Resource Filter
- **Base Class:** IResourceFilter
-  **Use as:** program.cs => `.Add<CustomHeaderFilter>`
- قبل از همه ی فیلترها اجرا می شود.
- موارد مصرف: Global Exception Handling، Response Modification، Global Logging، Request Preprocessing

### Action Filter
- **Base class:** IActionFilter
- **Use as:** `[ServiceFilter(typeof(CustomFilter))]`
- قبل و بعد از اجرای درخواست ها انی فیلتر اجرا می شود.

### Result Filter
- **Base class:** IResultFilter
- **Use as:** `[ServiceFilter(typeof(CustomFilter))]` - `.Add<CustomFilter>`
- قبل از اجرای اکشن متد و قبل از پردازش ریزالت خروجی اجرا می شود.
- موارد مصرف: تغییر خروجی درخواست، افزودن هدر خاص به درخواست ها

### Exception Filter
- **Base Class:** IexceptionFilter
- **Use as:** `[ServiceFilter(typeof(CustomFilter))]` - `.Add<CustomFilter>`

#### منابع:
- [منبع 1](https://www.c-sharpcorner.com/article/action-filters-in-asp-net-core/)
- [منبع 2](https://dotnettutorials.net/lesson/difference-between-typefilter-and-servicefilter-in-asp-net-core-mvc/)
