# How to integrate 9Chau IOS SDK for PKTK Game

##### Ver. 1.1.0 (28/08/2015)

#### Table of Contents

[0. Requirements](#0-requirements)

[1. Import 9chauSDK](#1-import-9chausdk)

[2. Import the important frameworks](#2-import-the-important-frameworks)

[3. Config project properties](#3-config-project-properties)

[4. Config URL types](#4-config-url-types)

[5. Prepare for using CuuChauSDK](#5-prepare-for-using-cuuchausdk)

[6. Add authentication function](#6-add-authentication-function)

[7. Add payment function](#7-add-payment-function)

[8. Add profile function](#8-add-profile-function)

[9. Add tracking play function](#9-add-tracking-play-function)

### 0. Requirements

##### 0.1. Development Environment

- Tool: using Xcode is the best.
- Minimum Deployment target: iOS 6.0.

### 1. Import 9chauSDK

##### 1.1. Extract file *CuuChauSDK.zip*, we get a folder named *CuuChauSDK*. 

![Alt text](http://i.imgur.com/nqczPxD.png?1 "")

##### 1.2. Copy two children folders named *include* and *lib* to your project folder as below:

![Alt text](http://i.imgur.com/JoZAjIL.png?1 "")

##### 1.3. Drag that folders into your project in Xcode and choose options like that:

![Alt text](http://i.imgur.com/azNkyt8.png "")

and the result after importing in Xcode:

![Alt text](http://i.imgur.com/QdnZPGO.png "")

### 2. Import *the important frameworks*

##### 2.1. In folder *CuuChauSDK* extracted before, copy the children folder named *Frameworks* to your project folder as below:

![Alt text](http://i.imgur.com/q8SA02Z.png?1 "")

##### 2.2. Drag *Frameworks* folder in to your project in Xcode and choose options as *step 1*, and the result like that:
 
![Alt text](http://i.imgur.com/9zU4ryC.png "")

### 3. Config project properties

##### 3.1. Click on the *DemoSDK* project in the project navigator, then select the *DemoSDK* target. Switch to the *Info tab*. Click on children tab *Custom iOS Target Properties*. Right-mouse click on any properties and choose Add Row to add those *important* properties:

![Alt text](http://i.imgur.com/0q4Y7E6.png "")

##### 3.2. Add game_code property:
- Key: **game_code**
- Type: **String**
- Value: **your_game_code** (fgame-pk-truyen-ky)

![Alt text](http://i.imgur.com/BRNLOjY.png "")

##### 3.3. Add FacebookAppID property:
- Key: **FacebookAppID**
- Type: **String**
- Value: **1015415611823872**
 
![Alt text](http://i.imgur.com/A2QZZrM.png "")

##### 3.4. Add FacebookDisplayName property:
- Key: **FacebookDisplayName**
- Type: **String**
- Value: **9chau**

![Alt text](http://i.imgur.com/nO4jleY.png "")

##### 3.5. Add FacebookUrlSchemeSuffix property:
- Key: **FacebookUrlSchemeSuffix**
- Type: **String**
- Value: **your_unique_scheme_suffix**
- Note: The *URL scheme suffix* **must start** with *a letter* and **only contain** *lowercase letters* and *numbers*.
 
![Alt text](http://i.imgur.com/qq1WktO.png "")

### 4. Config URL types

##### 4.1. Click on the *DemoSDK* project in the project navigator, then select the *DemoSDK* target. Switch to the Info tab. Click on children tab *URL Types* and click on the + button:

![Alt text](http://i.imgur.com/UXdh7lp.png?2 "")

##### 4.2. Assign URL Schemes equals *fb1015415611823872your_unique_scheme_suffix* as below:

![Alt text](http://i.imgur.com/d26cIAL.png "")

### 5. Prepare for using CuuChauSDK

To user the functions in CuuChauSDK, you need to conenct your AppDelegate to the EWayApplicationDelegate. Please follows the below guide step-by-step:

##### 5.1. Open file AppDelegate.m

##### 5.2. Import Eway.h header: 

```objc
	#import "EWay.h"
```
##### 5.3. In your ***AppDelegate.m*** add:

#### 5.3.1.

![Alt text](http://i.imgur.com/0BzptVp.png?1 "")

#### 5.3.2.

![Alt text](http://i.imgur.com/VIsAjk3.png "")

#### 5.3.3.

![Alt text](http://i.imgur.com/nXPiaE8.png "")

### 6. Add authentication function

##### 6.1. Import the header file Eway.h:

```objc
	#import "EWay.h"
```

##### 6.2. To show authentication function, use this script:

```objc
	[EWay authorizeWithCompleted:^(EWayUser *user, NSError *error) {
		if(!error) {
	     		//if authentication is success, your code here      
		} else {
	        	//if error, your code here
		}
	}];
```
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

If you want to get username property, you can access to **user** object by use this script: user.username;

### 7. Add payment function

##### 7.1. Import the header file Eway.h:

```objc
	#import "EWay.h"
```

##### 7.2. To show payment function, use this script to payment button:

```objc
	[EWay showRechargePanelWithGameOrder:(NSString *)gameOrder serverId:(NSString *)serverId andCompletedBlock:^{
		//if recharging is success
		//your code here, action to processing in your game (example: adding money for users)
	}];
```

Note 1: **gameOrder** is provided by game application when show payment view, and we return it into your game server ( by your API ) after recharge successfully.

Note 2: **gameOrder** is **json string**  and **optional**.

Note 3: **serverId** is **string** and you must pass it in this function to identify the server that users are playing.


### 8. Add profile function

##### 8.1. Import the header file Eway.h:

```objc
	#import "EWay.h"
```

##### 8.2. In file .m that you want to add profile button, at tag @interface implements EwayDelegate, example: 

```objc
	@interface ViewController ()<EWayDelegate>

	@end
```

##### 8.3. To show profile, use this script:

```objc
    UIApplication *application  = [UIApplication sharedApplication];
    UIWindow* window = application.keyWindow;
    if (!window || window.windowLevel != UIWindowLevelNormal) {
        for(window in [UIApplication sharedApplication].windows) {
            if (window.windowLevel == UIWindowLevelNormal) {
                [window makeKeyAndVisible];
                break;
            }
        }
    }
    
   	if ([EWay isAuthenticated]) {
		//if authenticated, allow to show profile button
	    [EWay setDelegate:self];
	    [EWay addDashboardButtonInView:window atPoint:CGPointMake(25.0, 100.0) withSize:CGSizeMake(50.0, 50.0) canMove:YES];
   	} else {
		//if not, your processing code here
	}
```

##### 8.4. And if you want to use 9Chau logout function, in this file, implement the below function:

```objc

	- (void)eWayDidSignOut {
		//your code here to processing log out
	}

```

### 9. Add tracking play function

Please add tracking play function after user choose game server:

##### 9.1. Import the header file Eway.h:

```objc
	#import "EWay.h"
```

##### 9.2. To tracking player, use this script:

```objc
	[EWay trackingPlayWithServerId:(NSString *)serverId andCompletedBlock:^{
            
        }];
```

Note: **serverId** is **string**.
