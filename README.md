# HCI-Android-Unlocking-
That's for HCI Class

Getting started

This whole tutorial assumes we want to create a free application published under the package name com.myapp and an unlocker application published under the package name com.myapp.unlocker.

Create the required projects

Create a library project for your application, that will be shared between your free application and the unlocker application. That project should depend on the library project you created in step 1.
Create an application project for the unlocker. That project should depend on the library project you created in step 2.
Create an application project for the free app. That project should depend on the library project you created in step 2.
Implement the app's common library project

This is the project that will contain the code needed to unlock the features in the free application and also the code that is needed to check if the unlock application is installed or not.

Copy the android-unlocker-library-1.0.0.jar file into the libs folder.
Create a class Configuration in the package com.myapp.common. You can take the sample class provided in the sample-common project. Make sure you replace the PACKAGE_NAME constant with your free application's package name. In our case com.myapp. As authority, you can safely use your package name, but you can use anything you want. Just remember to change it accordingly in the UnlockerProvider class constructor and in the manifest file of the unlocker application.
Create an UnlockerProvider class in the package com.myapp.common.provider that will handle the authorisations. You can take the sample class provided in the sample-common project. This class should extend the AuthorizationContentProvider class. This is where you can define how features are made available or not.
That should be it for the code in common for the unlocker and the free application.

Implement the unlocker application

That one is easy. In fact the unlocker application is nothing more than some icons and a few lines in the AndroidManifest.xml file. In that file, we request a permission to access the unlocker's content provider and we will also declare it so that it is available to our free application.

Add the icons in the drawable folders as usual
Name your application properly (usually automatically created in the strings.xml file)
Declare a new permission in the manifest file. You can use the sample declaration from the sample-unlocker project. Just replace the fr.marvinlabs.samples.unlocker.AUTHORIZATION_PROVIDER string with your own. In our case, we will name it com.myapp.AUTHORIZATION_PROVIDER.
Add the content provider declaration within the application tag of the manifest file. You can use the sample declaration from the sample-unlocker project. Just replace the fr.marvinlabs.samples.unlocker.common.provider.SampleUnlockerProvider string with your own. In our case, we will name it com.myapp.common.provider.UnlockerProvider and also replace the fr.marvinlabs.samples.unlocker.AUTHORIZATION_PROVIDER string with your own. In our case, we will name it com.myapp.AUTHORIZATION_PROVIDER.
In the end, your manifest file should look like that:

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.myapp.unlocker"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="14"
        android:targetSdkVersion="18" />

    <permission
        android:name="com.myapp.AUTHORIZATION_PROVIDER"
        android:protectionLevel="signature" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <provider
            android:name="com.myapp.common.provider.UnlockerProvider"
            android:authorities="com.myapp"
            android:permission="com.myapp.AUTHORIZATION_PROVIDER" />
    </application>
</manifest>
Implement the free application

This will be where you will want to check if the user has installed the unlocker application. In order for that to work, you will need to:

Request the permission to use the unlock content provider declared in the unlocker. This is done by adding a uses-permission tag in the AndroidManifest.xml file. You can use the sample declaration from the sample-locked project. Just replace the permission name with the one you declared in the unlocker project. In our case, we could add the line <uses-permission android:name="com.myapp.AUTHORIZATION_PROVIDER" /> within the manifest tag. That permission will not show up when the user installs the application because it is not a system-level permission, so no worries.
That's it, now you simply need to check whether the user has installed the unlocker application by using such code:

boolean isAuthorized = UnlockerProvider.getPackageLevelAuthorization(getContentResolver());
