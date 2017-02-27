# Ionic-docker
A ionic 2 image to be used with Gitlab CI

### Features
- Node 6.9.1
- Npm 3.10.8
- Ionic 2.1.12
- Cordova 5.3.1 
- android-23
- Ready to run Google Chrome for e2e tests
- Ruby 2.2 (usefull for scss-lint)
- Yarn 0.17.3
- Android SDK (build tools, platform tools, android-23)

## Usage

```
docker run -ti --rm -p 8100:8100 -p 35729:35729 fallen90/ionic-docker
```
If you have your own ionic sources, you can launch it with:

```
docker run -ti --rm -p 8100:8100 -p 35729:35729 -v /path/to/your/ionic-project/:/myApp:rw fallen90/ionic-docker
```

### Automation
With this alias:

```
alias ionic="docker run -ti --rm -p 8100:8100 -p 35729:35729 --privileged -v /dev/bus/usb:/dev/bus/usb -v ~/.gradle:/root/.gradle -v \$PWD:/myApp:rw fallen90/ionic-docker ionic"
```

> Due to a bug in ionic, if you want to use ionic serve, you have to use --net host option :

```
alias ionic="docker run -ti --rm --net host --privileged -v /dev/bus/usb:/dev/bus/usb -v ~/.gradle:/root/.gradle -v \$PWD:/myApp:rw fallen90/ionic-docker ionic"
```

> Know you need gradle for android, I suggest to mount ~/.gradle into /root/.gradle to avoid downloading the whole planet again and again

you can follow the [ionic tutorial](http://ionicframework.com/getting-started/) (except for the ios part...) without having to install ionic nor cordova nor nodejs on your computer.

```bash
ionic start myApp tabs
cd myApp
ionic serve
# If you didn't used --net host, be sure to chose the ip address, not localhost, or you would not be able to use it
```
open http://localhost:8100 and everything works.

### Android tests
You can test on your android device, just make sure that debugging is enabled.

```bash
cd myApp
ionic platform add android
ionic build android
ionic run android
```

## FAQ
    * The application is not installed on my android device
        * Try `docker run -ti --rm -p 8100:8100 -p 35729:35729 --privileged -v /dev/bus/usb:/dev/bus/usb -v \$PWD:/myApp:rw fallen90/ionic-docker adb devices` your device should appear
    * The adb devices show nothing whereas I can see it when I do `adb devices` on my computer
        * You can't have adb inside and outside docker at the same time, be sure to `adb kill-server` on your computer before using this image