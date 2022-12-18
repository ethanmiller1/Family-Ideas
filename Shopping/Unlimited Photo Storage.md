# Google Pixel Unlimited Storage Deal

* [How to Get Unlimited Google Photos Storage for Life](https://www.androidpolice.com/2021/06/02/how-to-google-photos-pixel-free/)

## Supported Phones

1. Google Pixel
2. Google Pixel XL

You can either [buy the phone](https://www.ebay.com/sch/i.html?_dcat=9355&_fsrp=1&_from=R40&_ftrt=901&_salic=1&_ipg=60&_in_kw=1&_dmd=1&_sargn=-1%26saslc%3D1&_stpos=75098&_nkw=Google+Pixel&_sacat=0&Storage%2520Capacity=128%2520GB%7C64%2520GB&_ex_kw=case%2C+protective%2C+protector%2C+degoogled&_ftrv=1&_udhi=70&Model=Google%2520Pixel%7CGoogle%2520Pixel%2520XL&_sop=1&rt=nc&LH_PrefLoc=1) or use an emulator.

# Emulator

1. Download [Android Studio](https://developer.android.com/studio).
2. Open AVD Manager
3. Choose one of the supported Pixel Phones
4. Click "Launch this AVD in the emulator"

You will have to sign in with your Google account, but the emulator should save the state when you turn it off.

![[Pasted image 20221217115018.png]]

1. Drag and drop files to your emulator
2. Click Device Folders to discover photos under Downloads
3. Backup the photos
4. Free up space

## Doesn't work for videos

https://thomas.vanhoutte.be/miniblog/google-photos-upload-error/

![[Pasted image 20221217185134.png]]

## Online

Work around it by manually uploading that big video file through the web interface at [https://photos.google.com/](https://photos.google.com/).

## Move file location

On my phone (Galaxy S6 on Lollipop 5.1.1) I moved the offending image (a photo, no different than all the others that sync correctly) to another folder using Samsung’s My Files app. I then cleared the cached app data and moved the photo back into the Camera folder. It worked! Google Photos synced the file immediately.