1. توضیح معنا دار در commit بنویسیم.
2. اگر انگلیسی است ابتدای جمله با حروف بزرگ شروع شود.
3. حداکثر در 50 کاراکتر باشد.
4. اگر توضیحات بیشتری برای توضیح کامیت نیاز است و یا نیاز به درج url خارجی است در توضیحات ایشو در خط دیگری description اضافه کنید.
برای این کار کافی است که بعد از درج متن کامیت مجددا برای درج توضیحات `m-` دیگری بگذارید:
```
git commit -m "Title" -m "Description .........."
```
5. اگر commit برای شماره ایشوی خاصی است در انتهای متن commit با استفاده از کلماتی مانند fixes، resolve یا close و درج شماره  ایشو به آن ایشو ارجاع دهیم. با این کار میتوان از قابلیت های گیت برای بستن یا Done کردن ایشو به طور خودکار استفاده کرد:

```
git commit -m "Add appearance class to Order, resolve #12"
```
6. به ازای هر تغییر منطقی کوچک در کد کامیت بزنیم، نگذاریم تغییرات زیاد شود و سپس کامیت کنیم.
7.  اگر تغییرات با همکاری کسی انجام شده می توان نام آن فرد را نیز در کامیت درج کرد. برای این کار کافی است که بعد از درج متن کامیت علامت `"` را نگذاشته و دو بار enter بزنید و سپس توضیحات مورد نظر را اضافه کنید:

```
git commit -m "Refactor usability tests.
>
>
Co-authored-by: NAME <NAME@EXAMPLE.COM>
Co-authored-by: ANOTHER-NAME <ANOTHER-NAME@EXAMPLE.COM>"
```
8. در انتهای متن کامیت از `. ` استفاده نکنید.
9. متن جمله ی کایمت بایستی امری باشد:
```
Add appearancee for signature
Remove pdf condition for versioning
```
10. برای Merge Requestها نیز کامیت بنویسید.
11. کامیت برای مرج ریکوئست ها باید تاثیر مرج ریکوئست را نشان دهد. مانند:
```
If applied, this commit will refactor subsystem X for readability
If applied, this commit will update getting started documentation
If applied, this commit will remove deprecated methods
```
12. نوشتن پیش کلمات کلیدی برای کامیت توصیه می شود. یعنی با توجه به نوع کامیت از کلمات کلیدی استفاده گردد. برای خودکار سازی بعضی امکانات گیت این کلمات کلیدی می توانند مورد استفاده قرار بگیرند.

| کلمه کلیدی | معنی |
| ------ | ------ |
| feat: | ویژگی جدید |
| fix: | رفع باگ |
| chore: | حذف کدهای اضافی و تمیزکاری های پروژه |
| ci: | تغییراتی که مربوط به CI/CD می شود |
| docs: | تغییرات مربوط به مستند سازی |
| refactor: | ریفکتور |
| perf: | بهبود پرفورمنس |
| test: | یونیت تست و سایر تست نویسی ها |

```
feat: Add new endpoint for static images [DHIS2-7656]
fix: Expire user sessions at password change [DHIS2-7355]
chore: Clean up code for attribute value service
```



---
منابع:

- [8 نکته کوتاه برای کامیت بهتر](https://medium.com/@saeid/10-essential-practices-for-better-git-commits-and-why-they-matter-3cfc420bf53e)
- [نکاتی در بابت کامیت و کامیت در مرج ریکوئست](https://cbea.ms/git-commit/)
- [کلمات کلیدی مورد استفاده در کامیت](https://github.com/dhis2/wow-backend/blob/master/guides/git_commit_messages.md)
