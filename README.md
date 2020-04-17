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

<b>Clean the entire build directory [/aosp_root/out/] by:

aosp_root$ make clean

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
