# 9Chau SDK integration for PKTK Game

##### Ver. 1.1.1 (24/07/2015)



#### Table of Contents
[1. Introduction](#1-introduction)

[2. Terminology](#2-terminology)

[3. Mind map and Functions](#3-mind-map-and-functions)
    
[3.1. Authentication](#31-authentication-before-playing-game-user-must-pass-by-sdk-authentication)

[3.2. Payment](#32-payment)
    
[4. Requirements](#4-requirements)
    
[4.1. Development Environment](#41-development-environment)
    
[5. Important Notes](#5-important-notes)

[6. How to integrate SDK](#6-how-to-integrate-sdk)

[6.1. Import 9chauSDK by Android Studio](#61-import-9chausdk-by-android-studio-recommended)

[6.2. Import 9chauSDK by Eclipse](#62-import-9chausdk-by-eclipse)

[6.3. Initialize SDK](#63-initialize-sdk)

[6.4. Add authentication function](#64-add-authentication-function)

[6.5. Add payment function](#65-add-payment-function)

[6.6. Add profile function](#66-add-profile-function)




### 1. Introduction
This guide is intended to help you integrating SDK for Game. Please read this guide carefully while integrating.

### 2. Terminology
-	A 9Chau account is an account in 9Chau system.
-	A game account, user must register and login before play game.
-	A Telco is telecom company: Viettel, Vinaphone, Mobiphone, …
	- A Telco card contain two code: serial and pin code. User must enter completely and accurately when using payment function.
-	A transaction is a game order when user recharge to a game.
- 	A transaction ID is unique order ID for each game order.


### 3. Mind map and Functions

9Chau SKD contain tree major components: Authentication, Payment, Profile Management.


#### 3.1. Authentication: Before playing game, user must pass by SDK authentication.

-	If user want to play game immediately, they can choose **Play now** function. SDK create a random UID for this device, this UID is unique ID on this device. Each device only create one UID and user still use this UID for each other play time.
-	If user has Facebook account, they can choose **Login by Facebook** function. SDK syncs Facebook ID from Facebook App to the game.
-	If user want to use **9Chau account**, they can register and login by 9Chau login function. After that, they must create game account or choose account they created before.


#### 3.2. Payment:
- Telco Card Payment: User must enter completely and accurately serial code and pin code when recharging to game.
- C Payment: C is money unit in 9Chau system, user can use C for recharging to game.
- Google wallet: recharging by google payment service


![Alt text](http://i.imgur.com/L0SBnud.png "9Chau SDK Mind Map")

*9Chau SDK Mind Map*



### 4. Requirements
Please make sure development environment and your game meet the following requirements before integrating SDK.

#### 4.1. Development Environment
- For Android development Platform Version 4.0 / API Level 14 is the minimum supported platform.
- You also can use any Android development tool for integrating SDK. But using **Android Studio Tool** (1.2.0 is current version) is the best. 



    
    
    


### 5. Important Notes

- Only using exact SDK for each game. If using another SDK, we do not guarantee that data returns correctly.
- Please note that the data accumulated during SDK testing is accumulated as actual measurement data.
- Using authentication function by my 9Chau system.
- Using real Telo card for testing payment function.



    
    
    


### 6. How to integrate SDK
#### 6.1. Import 9chauSDK By Eclipse
##### 1.	Download **eclipse_9chausdk.zip** to your computer and extract it.
##### 2.	Import the library project into your Eclipse workspace. Click **File** > **Import**, select **Android** > **Existing Android Code into Workspace**, and browse to the library folder you extracted to import it: 
![Alt text](http://i.imgur.com/OvSpAzm.png?1 "For Eclipse")
##### 3.	In your app project, reference Google Play services library project:
 
a.	In the **Package Explorer**, right-click the dependent project and select **Properties**.

b.	In the **Properties** window, select the "**Android**" properties group at left and locate the **Library** properties at right.

c.	Click **Add** to open the **Project Selection** dialog.

d.	From the list of available library projects, select **EclipseSdk** project and click **OK**.

e.	When the dialog closes, click **Apply** in the **Properties** window.

f.	Click **OK** to close the **Properties** window.



![Alt text](http://i.imgur.com/jq1L5ZE.png?1 "For Eclipse")


##### 4. 	Config project 

Add exact these config into **application** tag in your **AndroidManifest.xml**:

```java
	<meta-data
            android:name="com.facebook.sdk.ApplicationId"
            android:value="@string/facebook_app_id" />
      	<meta-data
            android:name="game_code"
            android:value="fgame-pk-truyen-ky" />
      	<meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
            
        <service android:name="com.cuuchau.sdk9chau.HUD"/>

```


##### 5. 	Add new BroadcastReceiver 

An Android application cannot have multiple receivers which have the same intent-filtered action. If you want have more than one INSTALL_REFFERER receiver, you must make the proxy receiver like this:

- Remove all INSTALL_REFERRER receiver.
- Add this receiver into your **AndroidManifest.xml**

```java
        <receiver
            android:name="{your_package_name}.tracking.Install"
            android:exported="true" >
            <intent-filter>
                <action android:name="com.android.vending.INSTALL_REFERRER" />
            </intent-filter>
        </receiver>
```
    
- Add this permission to your **AndroidManifest.xml**:

```java
	<uses-permission android:name="android.permission.INTERNET" />
   	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    	<uses-permission android:name="android.permission.READ_PHONE_STATE" />
    	<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
    	<uses-permission android:name="android.permission.GET_TASKS" />

    	<uses-permission android:name="android.permission.WAKE_LOCK" />
    	<uses-permission android:name="android.permission.VIBRATE" />
    	<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    	<uses-permission android:name="android.permission.GET_ACCOUNTS" />
    	<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

```
    
- Create package name is **tracking**, then create **Install.java** in this package:
    
```java
	public class Install extends BroadcastReceiver {
	    @Override
	    public void onReceive(Context context, Intent intent) {
	        com.cuuchau.sdk9chau.tracking.InstallationReceiver installationReceiver = new com.cuuchau.sdk9chau.tracking.InstallationReceiver();
	        installationReceiver.onReceive(context,intent);
	
	
		// your code here
	    }
	}
```
- Add these services to application tag in your AndroidManifest.xml:
```java
	        <activity
            android:name="com.facebook.FacebookActivity"
            android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />

        <activity
            android:name="com.cuuchau.sdk9chau.ui.RechargePanel"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.TelcoRecharge"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.AuthPanel"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.CharacterPanel"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.ProfilePanel"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.PlayNowPanel"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.VerifyAccountPanel"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.ChangeCPanel"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.WalletPanel"
            android:theme="@style/sdk9chau" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.FGPayment"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"
            android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" >
        </activity>
        <activity
            android:name="com.cuuchau.sdk9chau.ui.FGProfile"
            android:theme="@style/sdk9chau" >
        </activity>


```
#### 6.3. Initialize SDK

Add CuuChauSdk.sdk Initialize(this) into onCreate method in your main activity.

**Sample code:**

```java
	@Override
	protected void onCreate(Bundle savedInstanceState) {
	// Add this line to your code
	    CuuChauSdk.sdkInitialize(this);
	}
```

#### 6.4. Add authentication function


	To show authentication function, add this script to your main activity, in **onCreate** method

**Sample code:**

```java
    CuuChauSdk.showAuthPanel(new AuthCallback() {
        @Override
        protected void onLoginSuccess(JSONObject user) {
        	//your code here
        }
    
        @Override
        protected void onRegisterSuccess(JSONObject user) {
    	    //your code here
        }
    
        @Override
        protected void onLogout() {
        	//your code here
        }
    });
```

- If you want to use logout function for yourself in game ( not by "Logout" button in SDK view ), Please use below script:
```java
CuuChauSdk.logout();
```

##### Methods of AuthCallback:

| Methods | Parameters | Description  |
|:--------|:---------|:-----|
| onLoginSuccess      | JSONObject | Called after user login successfully |
| onRegisterSuccess      | JSONObject      |   Called after user create new game account |
| onLogout |       |  Called when user sign out   |



##### User Properties:
|Properties | Type | Description |
|:---|:---|:---|
|status|String|**status** = 0 (successful), otherwise is not successful|
|message| String | describes **status** in detail |
|username| String |must match with **username** 9chau server provides|
|game_code| String |must match **game_code** 9chau server provides|
|token|	String |must match **token** 9chau server provides|
|error_code| String	| must match **error_code** game server provides|
|session_key| String | must match **session_key** game server provides|

If you want to get username property, you can access to **user** object by use this script: user.getString("username");


#### 6.5. Add payment function


To show payment function, add this script to payment button click event:

```java
    CuuChauSdk.showRechargePanel(gameOrder, serverID, new PaymentCallback() {
        @Override
        public void onSuccess(JSONObject paymentResponse) {
	        // your code here
        }
    });
```

*Note: type of gameOrder parameter is json string.*
*Note: type of serverID parameter is json string.*

**Sample code:**

```java
    @Override
    public void onClick(View v) {
        if(v.getId()== R.id.btnCharge){
            CuuChauSdk.showRechargePanel(gameOrder, serverID, new PaymentCallback() {
                @Override
                public void onSuccess(JSONObject paymentResponse) {
    		        // your code here
                }
            });
        }
    }
```
Note: gameOrder is provided by game application when show payment view, and we return into game server ( by your API ) after recharge successfully.

##### PaymentCallback Methods:

|Methods|Parameters|Description|
|:---|:---|:---|
|onSuccess| JSONObject |Called after recharging success|


#### 6.6. Add profile function

To show profile, please add this script to profile button: CuuChauSdk.showProfilePanel();

**Sample code:**

```java
    @Override
    public void onClick(View v) {
        if(v.getId()== R.id.btnProfile){
            CuuChauSdk.showProfilePanel();
        }
    }
```



---



##### Version Log
- Ver. 1.1.0 (22/06/2015)
- Ver. 1.1.1 (24/07/2015)
	- Added: Push Notification
	- Fixed bugs
- Ver. 1.2.0 (04/08/2015)
	- Added: Solution intergrate by Eclipse
- Ver. 1.2.1 (10/08/2015)
	- Added: Logout function for logout in game
