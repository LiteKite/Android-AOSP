# Android-AOSP

This repo has all the information about AOSP things.

## Requirements

<b>Hardware and Software Requirements for downloading and building AOSP:</b>

https://source.android.com/setup/build/requirements#hardware-requirements

## Preparing WorkStation for AOSP:

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

<b>TAG is the build Number that could be <android-10.0.0_r25> that you can choose from:</b>

https://source.android.com/setup/start/build-numbers

<b>Sync the source code:</b>

aosp_root$ repo sync -j16


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
