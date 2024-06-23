# قفل کردن در حیطه مهندسی نرم افزار
## چه چیزهایی می توانند قفل شوند؟
قفل کردن می تواند برای Thread و یا process باشد. این دو در دو معقوله تاثیر دارند:
- ناحیه ی تاثیر گذار (**Scope**): 
  - قفل process: کنترل دسترسی انحصاری (exclusive) بین منابع مشترک کلی Thread های یک Process از طریق قفل کردن process 
  - قفل thread: به منبع مشترک در لحظه فقط یک Thread دسترسی داشته باشد
- ارتباطات بین فرایندها (**IPC (Inter-Process Communication)**): 
  - قفل process: مناسب دسترسی هم زمان (coordinating ) به منابع یکسان کل thread های یک فرایند توسط قفل فرایند
  - قفل thread: برای مدیریت race condition و دسترسی هم زمان داده ها در بین threadهای هم زمان از قفل thread استفاده می شود.

## چند نوع قفل داریم؟
- **انحصاری (Exclusive):** اجازه خواندن و نوشتن به هیچی نمی دهد.
- **غیر انحصاری (None-Exclusive):** اجازه نوشتن به هیچکی نمی دهد ولی اجازه خواندن می دهد.

### انواع قفل های Exclusive
1. **Lock**

تازمانی که threadی داخل lock است، کسی دیگری نمی تواند وارد شود. کد داخل lock باید کوتاه بوده و در سریع ترین زمان اجرا شود.

2. **Monitor**

هر threadی که به ابجکت monitor برسد باید در صف منتظر بماند تا منبع ازاد شود و بتواند منبع را در اختیار بگیرد. Monitor کلاس های Enter، Exit، Wait و Pulse دارد.

3. **Mutex**

ابجکت mnutex می تواند نام مختلف داشته باشد و وقتی Threadی به آن نام آبجکت میرسد باید مدت زمانی که در mutex مربوطه تعریف شده صبر کند تا منبع ازاد شده و آن را در اختیار بگیرد. عملکرد کلی آن مانند Monitor و Lock است.

4. **Spinlock**

برخلاف قبلی ها که thread لاک میشه، اینجا thread در یک loop تکرار می شه تا زمانیکه بهش مجوز بدن. اسم دیگه این کلاس busy waiting است. از معایب آن این است که تا زمانیکه به thread دسترسی بدهند، منابع درگیر بوده و بار محاسباتی دارد.


#### منابع
- بخش Exclusive Locking در کتاب C# nutshell حدود صفحه 1305
- [how to use lock in c#](https://code-maze.com/csharp-locking-mechanism/)

### انواع قفل های None-Exclusive
1. **Semaphore**

برای محدود کردن تعداد threadهایی که می توانند به منابع مشترک دسترسی داشته باشند استفاده می شود. برخلاف _lock_ یا _mutex_ منابع در انحصار Semaphore نیست و هر threadی می تواند semaphore را آزاد کند. در حالی که در _lock_ یا _mutex_ وقتی threadی آن ها را می گرد باید خودش آزادشان کند.
 در Semaphore می توان از Async نیز استفاده کرد. 
همچنین semaphore دو مقدار اصلی دارد:
- پارامتر initial count: تعداد thread هایی که می توانند وارد بخش قفل شده شوند و بلاک نشوند.
- پارامتر maximum count: تعداد thread هایی که می توانند وارد بخش قفل شده شوند یا بلاک شوند.

2. **Reader-Writer Lock Slim**

دو تا قفل reader و writer داریم. وقتی threadی وارد writer می شود همه ی threadهای دیگر چه خواندنی چه نوشتنی بلاک می شنود. وقتی هم که قفل writer باز است همه threadهای خواندنی یا نوشتنی می توانند منابع را در اختیار بگیرند.

#### منابع
- بخش Exclusive Locking در کتاب C# nutshell حدود صفحه 1305
- بخش None Exclusive Locking در کتاب C# nutshell حدود صفحه 1324
- [how to use lock in c#](https://code-maze.com/csharp-locking-mechanism/)

---

# تحقیقات مربوط به lock در .NET
- با توجه به توضیحات گفته شده نوع قفل Semaphore با امکان اجرای تسک های Async مناسب کار ماست اما در دات نت دو کلاس با نام های **Semaphore** و **SemaphoreSlim** وجود دارد. لاک SemaphoreSlim نسخه سبک تر و بهینه تر از Semaphore است که بعد از دات نت 4 به آن افزودن شده است.

- برخلاف Semaphore که امکان دارد هر Threadی آن را آزاد کن بدون در نظر گرفتن اینکه آیا همان Thread آن semaphore را قفل کرده یا خیر، در دات این ویژگی برای Semaphore وجود ندارد و تنها Threadی که semaphore را در اختیار گرفته می تواند آن را ازاد کند.

- قفل SemaphoreSlim یک تابع `Wait(int milisecondTimedOut)` داراست که در آن عدد مشخص شده میگوید که وقتی threadی این تابع را فراخوانی کرد، اگر Semaphore آزاد است، بلافاصله آن را به Thread اختصاص داده و مقدار 1 را برمیگرداند. اگر semaphore آزاد نباشد به مقدار مشخص شده در پرانتز منتظر می ماند و اگر قفل آزاد نشد، سپس باز میگردد که تابع مقدار 0 را برمیگرداند. بنابراین اجرای تابع با مقدار 0 یعنی انتظاری وجود نداشته باشد و هان لحظه بازگردد.
```
The "Wait" command is a "try to obtain the mutex within x miliseconds". If the mutex is available, there is no wait and the command will return immediately to enter the mutex. It's better to read it as "TryEnter" with a wait of up to x miliseconds.
```

- مسئله دیگر استفاده از data structure مناسب برای ذخیره موقت شماره TransactionId و قفل های هر کدام است. با توجه به تحقیقات یکی از راسج ترین روش های استفاده از **ConcurrentDictionary** است. این نوع دیکشنری برای سناریوهای multi-thread مناسب است که چند thrad جهت دسترسی به دیتای دیکشنری با هم رقابت میکنند.(مانند سناریو فعلی ما) که consistency داده ها حذف شود. این دیکشنری سبک و یکی از راه های راحت و پیشنهاد شده برای استفاده توسط SemaphoreSlim است.
- 
شبه کد قفل کردن و آزاد کردن انتیتی:

```
using System;
using System.Collections.Concurrent;
using System.Threading;

public class EntityLocker
{
    private readonly ConcurrentDictionary<int, SemaphoreSlim> _locks = new ConcurrentDictionary<int, SemaphoreSlim>();

    public void LockEntity(int entityId)
    {
        var semaphore = _locks.GetOrAdd(entityId, new SemaphoreSlim(1, 1));
        semaphore.Wait();
    }

    public void UnlockEntity(int entityId)
    {
        if (_locks.TryGetValue(entityId, out var semaphore))
        {
            semaphore.Release();
        }
        else
        {
            // Entity ID not found in the dictionary
            throw new InvalidOperationException($"Entity with ID {entityId} is not locked.");
        }
    }
}
```

شبه کد دسترسی به انتیتی:
```
public class EntityProcessor
{
    private readonly EntityLocker _entityLocker = new EntityLocker();

    public void ProcessEntity(int entityId)
    {
        _entityLocker.LockEntity(entityId);
        try
        {
            // Process the entity
        }
        finally
        {
            _entityLocker.UnlockEntity(entityId);
        }
    }
}

```

#### منابع
- [بررسی تفاوت Semaphore و SemaphoreSlim](https://medium.com/@uderbentoglu/parallel-programming-with-semaphoreslim-in-net-407b6a6ee673)
- [معرفی Semaphore و SemaphoreSlim از دیدگان مایکروسافت](https://learn.microsoft.com/en-us/dotnet/standard/threading/semaphore-and-semaphoreslim)
- [توضیح توابع SemaphoreSlim از دیدگاه مایکروسافت](https://learn.microsoft.com/en-us/dotnet/api/system.threading.semaphoreslim.wait?view=net-8.0)
- [مقایسه dictionary و ConcurrentDictionary](https://henriquesd.medium.com/dictionary-and-concurrentdictionary-in-c-1347105289f8)
