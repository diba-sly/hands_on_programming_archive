# انتشار برای اولین بار

1. آخرین تغییرات را دریافت می کنیم. بر روی آدرس مورد نظر می رویم و پروژه را clone می کنیم:
```
cd /home/diba.sly/
sudo git clone https://uri.gitlabl/folder1/services/my-project.git --branch 1.3.0 project-folder
```
2. به روش زیر در مسیر مورد نظر پابلیش مورد نظر را می سازیم:

```
dotnet publish -c release -o <path>
```

3. پروژه را روی پورت مورد نظر اجرا می کنیم:

**روش اول:**
```
pm2 start "dotnet my-project.dll --urls http://0.0.0.0:90 --name MyProjectName:90"
```

**روش دوم:**

1. در پوشه ی روت اصلی که پوشه ی پابلیش نیز موجود است فایل ecosystem را به روش زیر می سازیم:

```
pm2 init ecosystem.config.js
nano ecosystem.config.js
```
2. فایل ecosystem را مطابق زیر پر می کنیم:
```
module.exports = {
  apps : [
  {
    name: "name of service",
    cwd: "exact root of folder that contain the dll file",
    script: "name of dll file",
    env: {
      "ASPNETCORE_URLS":"http://0.0.0.0:port",
      "mode":"Development",
      "Mode":"Development",
    },
    output: 'CustomName-out.log',
    error: 'CustomName-error.log'
  }
  ]
}
```
3. فایل را اجرا می کنیم:
```
pm2 start ecosystem.config.js

```

# آپدیت پروژه

1. آخرین تغییرات را دریافت می کنیم. بر روی آدرس مورد نظر آخرین تغییرات را pull می کنیم:
```
cd /home/diba.sly/project-folder#
sudo git pull
sudo git checkout <tag>
sudo git pull
```

2. سرویس مورد نظر را متوقف می کنیم:

```pm2 stop <id>```

3. به روش زیر در مسیر مورد نظر پابلیش سرویس را می سازیم:

```
dotnet publish -c release -o <path>
```

4. سپس سرویس را مجددا اجرا می کنیم:

```
pm2 start <id>
```
