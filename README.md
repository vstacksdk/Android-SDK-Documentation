# 1. Quick Start
VStack is the communication platform which provides SDK, API to add chatting, calling features to the third-party mobile applications or websites.
This Quick Start guide will help you run a sample project powered by VStack as fast as possible. To run the sample project, you will need Android Studio installed.

```
1. Clone sample project from git hub: https://github.com/vstacksdk/android-sdk-sample.git
2. Open and run in Android Studio
```

# 2. Setup and installation
To integerate VStack SDK to your application, you need to create an VStack account. If you do not have an VStack account, sign up here: https://developer-vstack.vht.com.vn. After creating your account successfully, you need create a project. When a project is created, VStack provides you an application ID (appId) and a RSA key pair (public key is used on your application, private key is used on your server).

![VStack create account](http://az1.azstack.com/msg_guide/azstack_create_account.png "VStack create account")

VStack SDK is built and designed to be used with Android Studio. The following instructions will help you to integrate VStack into your application:
### 2.1. Adding AAR
Copy VStackSDK-version.aar (download from https://developer-vstack.vht.com.vn) to libs folder at the app level.
Navigate to build.gradle file at the app level and adding the following lines:
```
	defaultConfig {
        minSdkVersion 15
        multiDexEnabled true	// if necessary
    }

	repositories {
        flatDir {
            dirs 'libs'
        }
	}
	
	dexOptions {				// if necessary
        javaMaxHeapSize "4g"
    }

	dependencies {
		compile(name: 'VStackSDK-version', ext: 'aar')
		compile 'com.android.support:appcompat-v7:23.1.1'
		compile 'com.google.android.gms:play-services-maps:9.4.0'
		compile 'com.google.android.gms:play-services-gcm:9.4.0'
		compile 'com.google.android.gms:play-services-location:9.4.0'
		compile 'com.google.android.gms:play-services-plus:9.4.0'
		compile 'com.android.support:multidex:1.0.1'	// if necessary
	}
```

![Add AAR lib](http://az1.azstack.com/msg_guide/update_build_gradle.png "Add AAR lib")

multiDexEnabled is optional. If you encounter build errors that indicate your app has reached a limit of the Android app build architecture, you must configure multiDexEnabled is true and your Application is MultiDexApplication. This error is reported as follows: 

```
Conversion to Dalvik format failed:
Unable to execute dex: method ID not in [0, 0xffff]: 65536
```

```
trouble writing output:
Too many field references: 131000; max is 65536.
You may try using --multi-dex option.
```

#### Important: Recently, Android Studio has problems with Google Play Services. It causes applications which integrated Google Cloud Messaging SDK crashed. It also affects our SDK because we also use GCM for push notification. To avoid this problem, play-services-v9 must be used.

### 2.2. AndroidManifest.xml
#### Set up permissions and references
The VStack SDK requires some permissions and references (activities, gcm receivers, google map api key...) from your app's AndroidManifest.xml file. These permissions allow the SDK to monitor network state and receive Google Cloud Messaging messages... Below is an example with a com.example package; replace with your own package when merging with your own manifest.
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.vht.sample"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="15"
        android:targetSdkVersion="21" />

	<!-- VStack permissions -->
	
	<permission
        android:name="com.vht.sample.permission.MAPS_RECEIVE"
        android:protectionLevel="signature" />
	
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="com.vht.sample.permission.MAPS_RECEIVE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.GET_TASKS" />
	
	<!-- VStack permissions -->
		
    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name">
		
		<!-- VStack activities-->
		
        <activity
            android:name="com.vht.activity.ChatActivity"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"
            android:launchMode="singleTask"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light"
            android:windowSoftInputMode="stateHidden|adjustResize" />
        <activity
            android:name="com.vht.activity.ChatGroupActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:launchMode="singleTask"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light"
            android:windowSoftInputMode="stateHidden|adjustResize" />
        <activity
            android:name="com.vht.activity.CallFromActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.CallToActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.ViewPagerPhotoActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.NoActionBar" />
        <activity
            android:name="com.vht.activity.ReviewPhotoActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.NoActionBar" />
        <activity
            android:name="com.vht.activity.ConversationActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.GroupListActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.GroupInfoActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.SendLocationActivity"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.ShowLocationActivity"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.ForwardActivity"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.FileExplorerActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light"
            android:windowSoftInputMode="stateHidden|adjustResize" />
        <activity
            android:name="com.vht.activity.GunDrawActivity"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />
        <activity
            android:name="com.vht.activity.GunGGalleryActivity"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.GunGDirectoryActivity"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
        <activity
            android:name="com.vht.activity.SelectContactChatGroupActivity"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />
		<activity
            android:name="com.vht.activity.StickerDetailActivity"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light"></activity>
        <activity
            android:name="com.vht.activity.StickerListActivity"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light"></activity>
	<activity
            android:name="com.vht.activity.RecentDetailActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:screenOrientation="portrait"
            android:theme="@style/VStackTheme.Light" />

		<!-- VStack activities-->

		<!-- VStack meta-data-->
		
        <meta-data
            android:name="com.google.android.maps.v2.API_KEY"
            android:value="AIzaSyDuqAE6o4Uj45SVyQstieg00BxxSA8ICAI" />
        <meta-data
            android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
			
		<!-- VStack meta-data-->
    </application>
	
</manifest>
```

#### Set up Google Maps API key
VStack SDK has sending location function which using Google Maps Android API. It's required to set up Google Map API key. Refer to the following link: https://developers.google.com/maps/documentation/android-api/signup to get your own api key and replace it in AndroidManifest.

# 3. Connecting
### 3.1 Instantiate
Each application must register with VStack for 1 appID, 1 RSA key pair (public key is used on your application, private key is used on your server). For example: 
```
String appId = "8f6b6d607b7b6f5ec45b367c1c97ca68";   
String publicKey = "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEApxDkgYRgUr24soveQzHXE9GqQLi8Kv0KJtd35glW8UHry6Vy7o2fCGMNDTewKLrQmFLvAoMavIT+ZWKGG0yaARJQ93GduKmQ1C9UAgc3hLlHfW/YabwgENCkUdKtrnLZdx603wxxCfrmehP7LsqEp8BaHQJeTy4FLdCofTqNb8sR836Vk5CWoc11RoYsFJqV6htmxTgpxaHG3jJZOj15ran2rDSN7/yQc+nl8YIEeqmppYWupAe1/N3MkXwmTUPqgQUkXwPbYbuvHtLKLtOaHqSjQ88Vppjg0igZO6gTjL0vY8eumtePb61Ylv8JXzZ6Y+CXoE6x8/15rjG8jCMFvwIDAQAB"
```

The VStack SDK has a primary interface VStackClient for interacting with VStack service. You have to initialize VStackClient object in onCreate() method of your Application or your main Activity.. Only once instance of VStackClient should be instantiated and used all time:
```
VStackClient vStackClient = VStackClient.newInstance(Context context,String appId,String publicKey);
```

### 3.2 Listeners
The VStack SDK uses listener pattern to push specific events to your application. You must register 2 listeners VStackConnectListener, VStackUserListener when initialize VStackClient object.
```
vStackClient.registerConnectionListenter(VStackConnectListener instance)
vStackClient.registerUserListener(VStackUserListener instance)
```

### 3.3 Connect and authenticate
In order to a user to chat or call, you must authenticate them first. VStack will accept any unique String as a User ID (UIDs, email addresses, phone numbers, usernames, etc), which we call as vStackUserId. So you can use any new or existing User Management system.
The process is described in the following model:

![VStack init and authentication](http://az1.azstack.com/msg_guide/Android_Connect_Authenticate.png "VStack init and authentication")

#### Step 0
Go to https://developer-vstack.vht.com.vn
```
	a. Create project
	b. Generate keys
	c. Update Authentication URL which used to authenticate user between VStack and your backend.
```

#### Step 1
Initialize SDK with your appID, public key as described in 3.1

#### Step 2
Call  vStackClient.connect(String vStackUserId, String userCredentials, String name) method to start connecting, autheticating process
```
vStackClient.connect(String vStackUserId, String userCredentials, String name);

Parameter: 	vStackUserId: your user id on your system, as described above
			userCredentials: can be your password, token on your system. VStack will not use this information. It's forwared to your server to authenticate your user.
			name: optional, used to display on push notification.
```

You must register VStackConnectListener for listening connection,authentication events such as: connected, disconnected, connect failed...
```
vStackClient.registerConnectionListenter(new VStackConnectListener() {

			@Override
			public void onConnectionError(VStackClient client,
					
					VStackException e) {
				// Connect or authenticate unsuccessfully
			}

			@Override
			public void onConnectionDisconnected(VStackClient client) {
				// Disconnected from VStack service
			}

			@Override
			public void onConnectionConnected(VStackClient client) {
				// Connect and authenticate successfully, can start chatting, calling from here
			}
		});
```

### Step 3
After calling connect method, VStack SDK will encrypt the following string:
```
{"vStackUserID":"...", "userCredentials":"..."}
```
using RSA 2048 algorithm with your public key. The Identity Token is returned and will be sent to VStack server.

#### Step 4
VStack decrypts Identity Token using RSA 2048 algorithm with the private key generated at step 0. Then VStack will use the public key generated at step 0 to encrypt the following string:
```
{"vStackUserID":"...", "userCredentials":"...", "timestamp": ..., "appId":"...", "code":"..."}
```
"code" is generated as below:
```
md5(appId + "_" + timestamp + "_" + secret_code)
```
The encryption process returns Authentication Token. VStack will send the Authentication Token to your server via Authentication URL (updated at step 0).

#### Step 5
On your server side, you need decrypt Authentication Token receiving from VStack server to get the "code". Compare "code" with the following string:
```
md5(appId + "_" + timestamp + "_" + secret_code)
```
If they are the same, execute authentication with vStackUserID and userCredentials on your backend. Please see sample code here: https://github.com/vstacksdk/backend-example

### 3.4 VStackUserListener
VStackUserListener must to be registered when initialize VStackClient object. It's used to get basic information of app's user such as: name, avatar... to display on chat, call screen.

```
vStackClient.registerUserListener(new VStackUserListener() {

            @Override
            public void getUserInfo(List<String> vStackUserIds, int purpose) {
                // This code implemention is only for testing purpose
                // When going into production, you have to implement your own
                // web service to get your app's user info
				try {
                    JSONArray arrayContact = new JSONArray();
                    for (String vStackUserId : vStackUserIds) {
                        JSONObject ob = new JSONObject();
                        ob.put("vStackUserId", vStackUserId);
                        ob.put("name", name);
                        ob.put("avatar", avatar);
                        arrayContact.put(ob);
                    }
                    VStackClient.getInstance().getUserInfoComplete(arrayContact, purpose);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }

            @Override
            public void viewUserInfo(String appUserId) {

            }

            @Override
            public JSONArray getListFriend() {
				// This code implemention is only for testing purpose
                // When going into production, you have to implement your own
                // web service to get list user info
				JSONArray jsonArray = getListUserInfo();
                return jsonArray;
            }
        });
```

In the getUserInfo(List<String> vStackUserIds, int purpose) method, you need return users'information (including vStackUserId, name, avatar) equivalent to the list vStackUserIds via JSONArray. At the end of this medthod, you must call vStackClient.getUserInfoComplete(arrayContact, purpose) method. These information will be displayed on chat, call screen. You can check the sample for more detail how to implement this method.

### 3.5 VStackCallListener
VStackCallListener

```
vStackClient.setCallListener(new VStackCallListener() {
    @Override
    public void onCallDuration(int callId, String vStackUserId, long duration) {
	Log.d("Duration", vStackUserId +"--"+ duration);
    }

    @Override
    public void onCallRinging(int callId, String vStackUserId, int status) {
	Log.d("Ringing", vStackUserId +"--"+ status);
    }

    @Override
    public void onCallAnswer(int callId, String vStackUserId, int status) {
	Log.d("Answer", vStackUserId +"--"+ status);
    }

    @Override
    public void onCallEnd(int callId, String vStackUserId, int status) {
	Log.d("End", vStackUserId +"--"+ status);
    }
});
```

### 3.6 VStackMakeChatGroupListener
VStackMakeChatGroupListener

```
vStackClient.setVStackMakeChatGroupListener(new VStackMakeChatGroupListener() {
    @Override
    public void onMakeChatGroupComplete(int r, int groupId, String groupName, List<VStackContact> vStackContacts) {
  
    }
});
```

### 3.7 VStackRecentFragment
Call log 

```
```

# 4. API
### 4.1 Chat with a user
```
vStackClient.startChat(Context context, String vStackUserId, String name, String avatar);

Parameter: 	context: The context to start chat, required
			vStackUserId: identifier of app’s user, required
			name: name of app’s user, default “No name”, optional
			avatar: avatar of app’s user, optional
```

### 4.2 Call to a user
```
vStackClient.startCall(Context context, String vStackUserId, String name, String avatar, long time);

Parameter: 	context: The context to start call, required
			vStackUserId: identifier of app’s user, required
			name: name of app’s user, default “No name”, optional
			avatar: avatar of app’s user, optional
			time: time call limit, // if time = 0 no limit, time # 0 limit
```

### 4.3 Video call to a user
```
vStackClient.startVideoCall(Context context, String vStackUserId, String name, String avatar, long time);

Parameter: 	context: The context to start call, required
			vStackUserId: identifier of app’s user, required
			name: name of app’s user, default “No name”, optional
			avatar: avatar of app’s user, optional
			time: time call limit, // if time = 0 no limit, time # 0 limit
```

### 4.4 Create a chat group
```
vStackClient.createGroup(Context context);

Parameter: 	context: The context to create group, required
```
When creating a new group chat, the app navigates to select users screen. To make this screen work fine, you must implement the method getListFriend() of VStackUserListener when initialize VStack service. 
In getListFriend() method, the information of users (who you want to add to the group) is returned via JSONArray object. You can check the sample for more detail how to implement this method.

### 4.5 Update user's information
Update your info for push notification

```
vStackClient.updateMyInfo(newName);
```

### 4.6 Chat history
To show chat history, you can call this method

```
vStackClient.viewChatHistory(Context context);
```

or use VStackConversationFragment in your application.
Important: if you use VStackConversationFragment in your application, you must initialize VStackClient object before using.

### 4.7 Disconnect
Disconnect from VStack server

```
vStackClient.disconnect();
```

### 4.8 Logout
Disconnect from VStack server and clear all cached data on client

```
vStackClient.logout();
```

### 4.9 Create a chat group with fragment (VstackChatGroupFragment)


```
VStackClient.getInstance().createGroupWithChatFragment(Context context);
```
Parameter: 	context: The context to create group, required

When creating a new group chat successfully, have groupId in listener 3.6. After you call this method
```
startWithChatGroupFragment(Context context, int groupId, Class cls)
```
Parameter: 	context: The context to create group, required
		groupId: The id of Group, required
		cls: The Class add VstackChatGroupFragment ,required

# 5. Push notification
### 5.1 Create API project
We're using new Google Cloud Messaging for push notification. So you must create a Firebase project in the Firebase console for new Cloud Messaging projects.
To create a FireBase project, follows instructions from https://developers.google.com/cloud-messaging/android/client

After creating a new FireBase project, add Firebase to your Android app
![Add FireBase to Android app](http://az1.azstack.com/msg_guide/firebase_add_project.png "Add FireBase to Android app")

After adding app, download a google-services.json file.

### 5.2 Set up Google Cloud Messaging in VStack Dashboard
Go to Firebase project console, choose Project Settings ==> Cloud Messaging, you'll get project keys including Server key and Sender Id.
![Project keys](http://az1.azstack.com/msg_guide/firebase_key.png "Project keys")

Go to your project's Dashboard, choose Push -> Configure for Android -> Add credentials. Enter the Server key, project package name.
### 5.3 Set up Google Cloud Messaging in client code
Copy google-services.json (from 5.1) file into the app/ directory of your Android Studio project.

Add the dependency to your project-level build.gradle:
```
classpath 'com.google.gms:google-services:3.0.0'
```

Add the plugin to your app-level build.gradle:
```
apply plugin: 'com.google.gms.google-services'
```

Instantiate VStackClient with your Sender Id via Options object.
```
VStackOptions vOptions = new VStackOptions ();
vOptions.setGoogleCloudMessagingId(senderId);			
VStackClient vStackClient = VStackClient.newInstance(this, appId, publicKey, vOptions);
```

You have to declare some permissions for push notification in AndroidManifest.xml
```
<permission android:name="{your package name}.permission.C2D_MESSAGE"
    android:protectionLevel="signature" />
<uses-permission android:name="{your package name}.permission.C2D_MESSAGE" />
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
<uses-permission android:name="android.permission.GET_ACCOUNTS" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
```

Add receivers and services to AndroidManifest.xml. Note that you must create your own gcm listener service extends GcmListenerService to receive push message.
```
<!-- [START gcm_receiver] -->
<receiver
    android:name="com.google.android.gms.gcm.GcmReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <category android:name="{your package name}" />
    </intent-filter>
</receiver>
<!-- [END gcm_receiver] -->

<!-- [START gcm_service] -->
<service
    android:name="{your package}.MyGcmListenerService"
    android:exported="false" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    </intent-filter>
</service>
<service
    android:name="com.vht.VStackInstanceIDListenerService"
    android:exported="false" >
    <intent-filter>
        <action android:name="com.google.android.gms.iid.InstanceID" />
    </intent-filter>
</service>
<service
    android:name="com.vht.RegistrationIntentService"
    android:exported="false" />
<!-- [END gcm_service] -->
```

Finally, create your own service extends GcmListenerService:
```
public class MyGcmListenerService extends GcmListenerService {

    @Override
    public void onMessageReceived(String from, Bundle data) {
        // Implement connect VStack service here
		VStackClient.getInstance().showNotification(this, data);
    }
}
```

In onMessageReceived method, you need connect to VStack service as described in #3 and call VStackClient.getInstance().showNotification(context, data) method.

# 6. UI customization
VStack SDK allows developers to change some basic attributes of chat, call screen via VStackUIOptions object.

```
VStackUIOptions vUI = VStackUIOptions.getInstance();
```

### 6.1 Change the color of the actionbar, call background:

```
vUI.setHeaderColor(0xff4caf50);
```

### 6.2 Change the notification icon:

```
vUI.setNotificationIcon(R.mipmap.your_icon);
```

These methods need to be called only once time when you initialize VStack SDK
