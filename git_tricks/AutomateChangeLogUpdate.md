### توضیح
برای ایجاد خودکار changelog بعد از تگ زدن یا انتشار و ... گیت لب امکاناتی را ارائه داده.  البته پروژه های open source دیگری با شخصی سازی این ابزارهای ساده ی گیت لب آن ها را حرفه ای تر کرده اند. در ادامه به توضیح ابزار ساده ی گیت لب پرداخته می شود.

### روش خودکار سازی تولید changelog با استفاده از API Git
در این روش اطلاعات از کامیت های مرج ریکوئست خوانده می شود. بعد از ایجاد تگ، اطلاعات release note آماده و درج می شود و سپس این اطلاعات به عنوان change log درج می شوند.

#### 1- تنظیمات اولیه برای خودکار سازی
1. ایجاد یک runner که قابلیت کار کردن با docker را داشته باشد.
- ([راهنمای ایجاد و نصب رانر برای پروژه](https://gitlab.partdp.ir/digitalSignature/dotnet/modules/partpkelibrary-dotnet/-/runners/213/register?platform=windows)) 
2. ایجاد یک Access Token برای دسترسی به همه ی قسمت های پروژه
- [راهنمای ایجاد access token](https://docs.gitlab.com/ee/user/project/settings/project_access_tokens.html#create-a-project-access-token) 
- نام این توکن در پروژه های ما `APITOKEN` است. 
- دقت کنید که بعد از ساخت توکن مقدار آن را کپی کنید تا در مرحله ی بعد استفاده کنیم. 
- تنظیمات را مطابق تصویر زیر انتخاب کنید.

![1](https://github.com/diba-sly/hands_on_programming_archive/assets/135495538/edb090a4-7842-4231-a28a-5c50b7f9afc7)

3. ایجاد Variable با مقدار Access Token در پروژه
- [راهنمای ایجاد variable برای پروژه](https://docs.gitlab.com/ee/ci/variables/#for-a-project)
- در تنظیمات CI/CD پروژه، این Access Token را به عنوان variable با نام `CI_API_TOKEN` ذخیره می کنیم. 
- تنظیمات ساخت این variable مطابق تصویر زیر است.

![2](https://github.com/diba-sly/hands_on_programming_archive/assets/135495538/0c29eadc-63b2-42a6-b890-8ac29edfeedc)

#### 2- پیاده سازی خودکار سازی ثبت changelog
1. گیت لب یک `gitlab trailor` دارد که ساختار خاصی را به کامیت ها اضافه یا از آن ها میخواند. برای آن که در commit ها از _Gitlab trainlor_ بخواهیم کامیت مربوطه را به عنوان changelog استفاده کند بایستی در انتهای کامیت عبارت `Changelog: Added` را درج کرد. البته به جای Added میتوان کلمات کلیدی دیگری نیز استفاده کرد.

```
<Commit message subject>

<Commit message description>

Changelog: Added 
```

| کلید واژه | معنی |
| ------ | ------ |
| Added | ویژگی جدید |
| Changed | تغییرات |
| Removed | حذفیات |
| Fixed | رفع باگ |
| Security | امنیتی |
| Deprecated | ویژگی هایی که حذف خواهند شد | 

- این کامیت پس از ایجاد مرج ریکوئست در Tab overview مرج ریکوئست با کلیک روی دکمه ی edit commit قابل ویرایش است. در تصویر زیر توضیحات را مشاهده می کنید:

![Capture](https://github.com/diba-sly/hands_on_programming_archive/assets/135495538/2d27b616-2c89-490a-b9e6-cd43c83b089f)

2. ساخت فایل CAHANGELOG.md در ریپو اصلی پروژه
- [توضیحات ساختار change log](https://keepachangelog.com/en/1.1.0/) 
- در خط اول عبارت `# change log` و بعد دو خط خالی میگذاریم. 
- از حالا تگ های جدید در ادامه ی فایل لیست می شوند.
3. آپدیت فایل `.gitlab-ci.yml` 
- در اینجا باید 2 Stage داشته باشیم؛ یکی برای ایجاد release note از روی کامیت آخرین مرج ریکوئست، دیگری درج release note در changelog. 
- کد مربوطه مشابه زیر می باشد:

```
stages:
  - prepare
  - release

variables:
  PROJECT_TAG: $CI_COMMIT_TAG
  API_TOKEN: $CI_API_TOKEN
  PROJECT_ID: $CI_PROJECT_ID

default:
  tags:
    - dotnet

# ایجاد ریلیز نوت بر اساس تگ جدید از روی کامیت های آخرین تگ تا تگ فعلی
prepare_job:
  stage: prepare
  only:
    - /^[0-9]+\.[0-9]+\.[0-9]+$/
  script:
    - curl -H "PRIVATE-TOKEN:$CI_API_TOKEN" "$CI_API_V4_URL/projects/$CI_PROJECT_ID/repository/changelog?version=$CI_COMMIT_TAG&branch=master" | jq -r .notes > release_notes.md
  artifacts:
    paths:
    - release_notes.md

# آپدیت فایل changelog
update changelog:
  stage: release
  only:
    - /^[0-9]+\.[0-9]+\.[0-9]+$/
  script:
    - curl -H "PRIVATE-TOKEN:$CI_API_TOKEN" -X POST "$CI_API_V4_URL/projects/$CI_PROJECT_ID/repository/changelog?version=$CI_COMMIT_TAG&branch=master"

```
**توضیحات:**
- پارامتر _CI_API_TOKEN_ همان توکنی است که در قسمت 1 ساخت آن توضیح داده شد.
- در کد نوشته شده با عبارت `branch=master` تنظیم کرده ایم که فقط فایل changelog روی برنچ master را آپدیت کند


---
منابع:
- [سمپل و ساختار changelog](https://keepachangelog.com/en/1.0.0/)
- [مثال های گیت لب](https://docs.gitlab.com/ee/api/repositories.html#add-changelog-data-to-a-changelog-file)
- [پروژه ای که از امکان گیت لب استفاده کرده](https://gitlab.mpcdf.mpg.de/nomad-lab/nomad-FAIR/-/merge_requests/1016/diffs#diff-content-587d266bb27a4dc3022bbed44dfa19849df3044c)
- [آموزش گیت لب](https://about.gitlab.com/blog/2023/11/01/tutorial-automated-release-and-release-notes-with-gitlab/)
