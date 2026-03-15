
Flutter - Installation
This chapter will guide you through the installation of Flutter on your local computer in detail.

Installation in Windows
In this section, let us see how to install Flutter SDK and its requirement in a windows system.

Step 1 − Go to URL, https://flutter.dev/docs/get-started/install/windows and download the latest Flutter SDK. As of April 2019, the version is 1.2.1 and the file is flutter_windows_v1.2.1-stable.zip.

Step 2 − Unzip the zip archive in a folder, say C:\flutter\

Step 3 − Update the system path to include flutter bin directory.

Step 4 − Flutter provides a tool, flutter doctor to check that all the requirement of flutter development is met.

flutter doctor
Step 5 − Running the above command will analyze the system and show its report as shown below −

Doctor summary (to see all details, run flutter doctor -v):
[] Flutter (Channel stable, v1.2.1, on Microsoft Windows [Version
10.0.17134.706], locale en-US)
[] Android toolchain - develop for Android devices (Android SDK version
28.0.3)
[] Android Studio (version 3.2)
[] VS Code, 64-bit edition (version 1.29.1)
[!] Connected device
! No devices available
! Doctor found issues in 1 category.
The report says that all development tools are available but the device is not connected. We can fix this by connecting an android device through USB or starting an android emulator.

Step 6 − Install the latest Android SDK, if reported by flutter doctor

Step 7 − Install the latest Android Studio, if reported by flutter doctor

Step 8 − Start an android emulator or connect a real android device to the system.

Step 9 − Install Flutter and Dart plugin for Android Studio. It provides startup template to create new Flutter application, an option to run and debug Flutter application in the Android studio itself, etc.,

Open Android Studio.

Click File → Settings → Plugins.

Select the Flutter plugin and click Install.

Click Yes when prompted to install the Dart plugin.

Restart Android studio.

Installation in MacOS
To install Flutter on MacOS, you will have to follow the following steps −

Step 1 − Go to URL, https://flutter.dev/docs/get-started/install/macos and download latest Flutter SDK. As of April 2019, the version is 1.2.1 and the file is flutter_macos_v1.2.1- stable.zip.

Step 2 − Unzip the zip archive in a folder, say /path/to/flutter

Step 3 − Update the system path to include flutter bin directory (in ~/.bashrc file).

> export PATH = "$PATH:/path/to/flutter/bin"
Step 4 − Enable the updated path in the current session using below command and then verify it as well.

source ~/.bashrc
source $HOME/.bash_profile
echo $PATH
Flutter provides a tool, flutter doctor to check that all the requirement of flutter development is met. It is similar to the Windows counterpart.

Step 5 − Install latest XCode, if reported by flutter doctor

Step 6 − Install latest Android SDK, if reported by flutter doctor

Step 7 − Install latest Android Studio, if reported by flutter doctor

Step 8 − Start an android emulator or connect a real android device to the system to develop android application.

Step 9 − Open iOS simulator or connect a real iPhone device to the system to develop iOS application.

Step 10 − Install Flutter and Dart plugin for Android Studio. It provides the startup template to create a new Flutter application, option to run and debug Flutter application in the Android studio itself, etc.,

Open Android Studio

Click Preferences → Plugins

Select the Flutter plugin and click Install

Click Yes when prompted to install the Dart plugin.

Restart Android studio.

