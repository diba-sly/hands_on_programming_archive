## IIS Middleware
زماني كه درخواستي ارسال مي شود، اولين ميدلوير كه جلوي راه آن قرار داد اين ميدلوير است. (البته اگر gateway قبل از آن قرار نگرفته باشد). 
سپس درخواست را بر اساس url pattern و تنظيمات configuration به AP.Net runtime پاس مي دهد.

وظايف:
- Managing hosting environment
- Handling SSL/TLS
- Managing connection pool

## The ASP.Net Core Middleware
در اين مرحله ميدلوير هاي متفاوتي مي توانند باشند كه از جمله وظايف آنها اين است:
- Authentication
- Authorization
- request/response transformations
- logging
- caching
پس از اجراي همه ی میدلویر ها نوبت به میدلویر routing می رسد که درخواست را به controller یا Razor مربوطه انتقال می دهد.
### Routing Middleware
میدلویر routing کار های زیر را انجام می دهد:
1. ساختار URL را با لیست تمپلیت های rout تعریف شده در سیستم تطابق داده و بر اساس الگوی آن، endpointی که باید درخواست به آن برسد را مشخص می کند.
2. سپس پارامترهای  لازم را از URL استخراج می کند.
    پارامترهای استخراج شده را در rout data ذخیره می کند.
# ASP.Net Application endpoint
سه نوع اندپوینت برای Asp.net application وجود دارد:
- MVC method
- Razor Pages
- direct minimal API

