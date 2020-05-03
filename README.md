# Android-AOSP

This repo has all the information about AOSP things.

## Requirements

<b>Hardware and Software Requirements for downloading and building AOSP:</b>

https://source.android.com/setup/build/requirements#hardware-requirements

## Preparing WorkStation for AOSP

<b>Setup Build Environment - Install required packages for building aosp:</b>

https://source.android.com/setup/build/initializing

<b>Install Source Control Tools:</b>

https://source.android.com/setup/develop

<b>Install Repo Client:</b>

https://source.android.com/setup/build/downloading

## Initialize AOSP Source Repository

<b>Initialize and sync the code</b>

https://source.android.com/setup/start

<b>Initiate the Repo Source</b>

aosp_root$ repo init -u https://android.googlesource.com/platform/manifest -b [TAG]

TAG is the build Number that could be <android-10.0.0_r25> that you can choose from:

https://source.android.com/setup/start/build-numbers

## Sync the Repo

<b>Sync the repo source code:</b>

aosp_root$ repo sync -j16

<b>Sync only a specific project:</b>

aosp_root$ repo sync [project-dir]

[Project-dir] could be any directory like this - /aosp_root/packages/apps/Settings

## Resolving Repo Sync Errors

<b>If Repo fails to sync, then try the below:</b>

<b>Tweak TCP Setting:</b>

aosp_root$ sudo sysctl -w net.ipv4.tcp_window_scaling=0

<b>or</b>

aosp_root$ repo sync -j16 --force-sync

<b>or</b>

aosp_root$ repo sync -j16 --fail-fast

<b>Some known-issues while syncing AOSP:</b>

https://source.android.com/setup/build/known-issues

<b>Checking out all the projects in the repo:</b>

Normally, when repo sync completes, it automatically checkout all projects to the current branch mentioned in the repo manifest file. If it does not checkout properly, try below:

aosp_root$ repo sync -l

The above will checkout all projects locally.

<b>If not sure about the repo manifest branch set correctly, then try resetting the repo:</b>

aosp_root$ cd .repo/

aosp_root/.repo$ rm -rf manifest.xml

aosp_root/.repo$ rm -rf manifests

aosp_root/.repo$ cd ..

aosp_root$ repo forall -c git clean -xdf; repo forall -c git reset HEAD --hard; repo sync -j32;

<b>If any project in the repo fails to sync:</b>

<b>Delete the corrupt project:</b>

aosp_root$ cd .repo/

Find the [project].git inside /aosp_root/.repo

There will two [project].git in /aosp_root/.repo/projects/ and /aosp_root/.repo/project-objects/

Delete those two [project].git

<b>And, try sync the repo alone by:</b>

aosp_root$ repo sync [project]

[project] could be any directory like this - /aosp_root/packages/apps/Settings

<b>If the repo locked due to multiple sync at the same time, then:</b>

<b>Find the locked object [.lock]</b>

aosp_root$ cd .repo/

aosp_root/.repo$ find . -iname *.lock

<b>And delete all the locked objects:</b>

aosp_root$ rm *.lock

<b>Sync the repo after all...</b>

aosp_root$ repo sync -j16

## Building the repo

<b>Setup environment:</b>

aosp_root$ source build/envsetup.sh

<b>Choose build variant:</b>

aosp_root$ lunch

<b>And, hit enter, give the build variant number (or) its text as the input like:</b>

aosp_root$ 8 (or) aosp_root$ [build-variant]

<b>(or) give it straight away...</b>

aosp_root$ lunch [build-variant]

<b>Build the repo by:</b>

aosp_root$ make -j16

<b>Clean the entire build directory [/aosp_root/out/] by:</b>

aosp_root$ make clean

## Updating Framework APIs

<b>If you try to make (or) add changes in the Framework APIs, then you need to update the build as well</b>

aosp_root$ make update-api

<b>These APIs are recorded and updated on the below files:</b>

root_aosp/frameworks/base/api/current.txt

root_aosp/frameworks/base/api/test-current.txt

## Download Factory Images and AOSP Builds

1) <b>AOSP CI Builds ></b> https://ci.android.com/builds/branches/aosp-master/grid

2) <b>AOSP Factory Images ></b> https://developers.google.com/android/images

## Working on AOSP with Android Studio

<b>Run idegen.sh from aosp development tools</b>

aosp_root$ make idegen && development/tools/idegen/idegen.sh

The <b>android.ipr</b> will get generated on the /aosp_root/ path

<b>Go to Studio > Help > Edit Custom VM and increase the memory:</b>

-Xms1g
-Xmx5g

<b>Go to Studio > Help > Edit custom properties and increase file size limit:</b>

idea.max.intellisense.filesize=100000

Then, restart your IDE

<b>Go to /aosp_root/ path and right click and open the "android.ipr" file with Android Studio.</b>

After all the index process are done, go to "project structure > SDK" and create a new JDK (no_libraries) configuration.

Then remove all of the jar entries under the "Classpath" tab to access only the aosp core libraries.

## Changing Android Boot Animation

The <b>bootanimation.zip</b> file can be found in <b>system/media/</b> folder of an android powered device.

During build time, the <b>bootanimation.zip</b> file is copied to the <b>system/media/</b> directory. 

This can be done by Car.mk build file found in <b>/aosp_root/packages/services/Car/car_product/build/car.mk</b>

PRODUCT_COPY_FILES += \
    packages/services/Car/car_product/bootanimations/bootanimation-832.zip:system/media/bootanimation.zip

<b>The original "bootanimation.zip" file is located at:</b>

/aosp_root/packages/services/Car/car_product/bootanimations/bootanimation-832.zip

<b>Making your own bootanimation.zip file:</b>

1) Create a "new_folder" and go to that folder.

2) Create <b>part0</b> folder inside "/new_folder/" and include all your PNG Animation Sequence Images.

3) Create <b>desc.txt</b> file and edit the <b>desc.txt</b> file by following the animation format:

   width height frame-rate
   
   p loops pause folder-sequence
   
   
   <b>The example would be:</b>
   
   832 520 30
   
   p 1 100 part0

4) After this, "new_folder" will have <b>part0</b> folder with images and <b>desc.txt</b> file.

   new_folder$ ls
   
   part0 desc.txt
   
5) Create uncompressed format of bootanimation.zip file

   new_folder$ zip -r -0 bootanimation.zip .
   
6) Rename <b>bootanimation.zip</b> file as <b>bootanimation-832.zip</b> file.
   
7) Copy and replace the new <b>bootanimation-832.zip</b> file in <b>/aosp_root/packages/services/Car/car_product/bootanimations/</b> path

8) Build the aosp project and flash the new build image.

9) <b>You can easily test this bootanimation by:</b>

   $ adb root
   
   $ adb remount
   
   $ adb push bootanimation.zip /system/media/
   
10) <b>Make it executable:</b>

    $ adb shell chmod 666 /system/media/bootanimation.zip
   
11) Restart the device, you will see the bootanimation.

12) <b>If not working, then launch bootanimation via adb shell:</b>

    $ adb shell bootanimation
    
## Changing Android Boot Image:

1) Use any online portal and convert PNG Boot Logo file into <b>PPM (Portable Pixmap Format)</b> format.

2) Make sure that the image width and height are supported by the system. ex) 832 x 520

3) Reduce the number of colors to 224 using <b>ppmquant</b>

    $ ppmquant 224 custom_logo.ppm > custom_logo_224.ppm
    
4) Convert the image to ASCII format using <b>pnmnoraw</b>

    $ pnmnoraw custom_logo_224.ppm > custom_logo_ascii_224.ppm
    
5) The final image would be <b>custom_logo_ascii_224.ppm</b> and replace this in your android build system.

## References

1) <b>Embedded Android ></b> https://www.oreilly.com/library/view/embedded-android/9781449327958/ch04.html

2) <b>Building AOSP ></b> https://www.polidea.com/blog/How-to-Build-your-Own-Android-Based-on-AOSP

3) <b>Building Custom ROM ></b> https://www.talentica.com/blogs/build-custom-android-rom-using-android-open-source-projectaosp/

4) <b>How to build Android ROMs on Ubuntu ></b> https://www.digitalocean.com/community/tutorials/how-to-build-android-roms-on-ubuntu-16-04

5) <b>AOSP Part 1 by UDI COHEN ></b> http://blog.udinic.com/2014/05/24/aosp-part-1-get-the-code-using-the-manifest-and-repo

6) <b>AOSP Part 2 by UDI COHEN ></b> http://blog.udinic.com/2014/06/04/aosp-part-2-build-variants

7) <b>AOSP Part 3 by UDI COHEN ></b> http://blog.udinic.com/2014/07/24/aosp-part-3-developing-efficiently

8) <b>IDEGEN Development Tool from Google ></b> https://android.googlesource.com/platform/development/+/master/tools/idegen/README

9) <b>Setting up AOSP with IntelliJ ></b> https://shuhaowu.com/blog/setting_up_intellij_with_aosp_development.html

10) <b>AOSP Code Search by Google ></b> https://cs.android.com

11) <b>Changing Android Boot Image and Animation ></b> https://www.digi.com/resources/documentation/digidocs/90001546/task/android/t_faq_change_android_boot_images.htm

## License

~~~

Copyright 2020 Vignesh S

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, 
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

~~~
