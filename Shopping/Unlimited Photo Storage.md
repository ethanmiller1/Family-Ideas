# Google Pixel Unlimited Storage Deal

* [How to Get Unlimited Google Photos Storage for Life](https://www.androidpolice.com/2021/06/02/how-to-google-photos-pixel-free/)

## Supported Phones

1. Google Pixel
2. Google Pixel XL

You can either [buy the phone](https://www.ebay.com/sch/i.html?_dcat=9355&_fsrp=1&_from=R40&_ftrt=901&_salic=1&_ipg=60&_in_kw=1&_dmd=1&_sargn=-1%26saslc%3D1&_stpos=75098&_nkw=Google+Pixel&_sacat=0&Storage%2520Capacity=128%2520GB%7C64%2520GB&_ex_kw=case%2C+protective%2C+protector%2C+degoogled&_ftrv=1&_udhi=70&Model=Google%2520Pixel%7CGoogle%2520Pixel%2520XL&_sop=1&rt=nc&LH_PrefLoc=1) or use an emulator.

* [Wamatek](https://wamatek.com/product/google-pixel-xl/?attribute_pa_carrier=unlocked&attribute_pa_color=gray&attribute_pa_condition=good&attribute_pa_storage=32gb&gclid=EAIaIQobChMI6cih9fiK_AIVXBOtBh2nDwVWEAQYAyABEgJlEfD_BwE) | [BBB](https://www.bbb.org/us/mn/eden-prairie/profile/electronics-and-technology/wamatek-0704-1000026030)  

# Emulator

> Note: Google is smart enough to know that your device is an emulator. It will show you the unlimited storage notification, but any photos uploaded through an emulator will count towards your storage.

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

Work around it by manually uploading that big video file through the web interface at [https://photos.google.com/](https://photos.google.com/).

## Move file location

On my phone (Galaxy S6 on Lollipop 5.1.1) I moved the offending image (a photo, no different than all the others that sync correctly) to another folder using Samsung’s My Files app. I then cleared the cached app data and moved the photo back into the Camera folder. It worked! Google Photos synced the file immediately.

## Reasons for choosing Google Photos

1. To have easy access to all my photos across all my devices.
2. To have a reliable backup of all my photos in case of hard drive failures.
3. Because of Google Photos incredible auto-tagging / face recognition software that makes searching through thousands of photos for the one you're looking for incredibly easy.

## Is it worth my time?

Google Photos [Pricing](https://one.google.com/about/plans) :

| Storage | Price       | Cost Over 60 years |
| ------- | ----------- | ------------------ |
| 15 GB   | Free        | Free               |
| 100 GB  | $20 / year  | $1,200             |
| 200 GB  | $30 / year  | $1,800             |
| 2 TB    | $100 / year | $6,000             |

| Estimate Description                                                          | Hours    |
| ----------------------------------------------------------------------------- | -------- |
| Estimated number of hours for Research and Development                        | 16       |
| Estimated number of hours for uploading Mom's Catalogue of 24,000 photos      | 30       |
| Estimated number of hours for uploading iCloud photos each month for 60 years | 720      |
| Initial hourly payoff                                                         | <mark class="hltr-yellow">$130/hr</mark> | 
| Lifetime hourly payoff                                                        | $7.83/hr |

I work 2,,080 hours each year making a little over $50/hr. I could use those 46 initial hours allocated to R&D and manual work for learning algorithms to increase my hourly pay from $50/hr to $150/hr. 46 hours would be insufficient but useful. That amount of time is worth $2,300 at my current salary.

# Google Photos Query Language

Google Photos allows you to search for up to 5 photos my using the pipe | symbol as an "OR" condition. Use the following command line to display a list of files delimited by pipes:

```bash
$ files=`ls -p | grep -v / | tr '\n' '|'` && echo $files | sed 's/|/\n/5;P;D'
```

You can search for:
* Recently Added
* Device Files
* Utilities
* People
* Location
* Things
* Date
* File name
* File Properties
* Description
* Documents

Combine two of the values mentioned above to create a more specific search term (e.g. "2019 tree").

[Google photos search hacks: The less known tips on searching and locating your photos](https://www.nousis.com/technology/google-photos-search-hacks/)
