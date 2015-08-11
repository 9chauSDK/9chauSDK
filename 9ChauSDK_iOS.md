# How to integrate IOS SDK

####### Ver. 1.1.0 (11/08/2015)

####### Table of Contents

[1. Import 9chauSDK](#1-import-9chausdk)

[2. Import Facebook frameworks](#2-import-facebook-frameworks)

[3. Import SystemConfiguration.framework](#3-import-systemconfigurationframework)

[4. Config project properties](#4-config-project-properties)

[5. Config URL types](#5-config-url-types)

[6. Prepare for using CuuChauSDK](#6-prepare-for-using-cuuchausdk)

[7. Add authentication function](#7-add-authentication-function)

[8. Add payment function](#8-add-payment-function)

[9. Add profile function](#9-add-profile-function)


### 1. Import 9chauSDK

###### 1.1. Extract file CuuChauSDK.zip, we get a folder named CuuChauSDK. Copy two children folders named include and lib to your project folder as below:


![Alt text](http://i.imgur.com/UHAoNaO.png "")

###### 1.2. Drag that folders into your project in Xcode and choose options like that:

![Alt text](http://i.imgur.com/azNkyt8.png "")

and the result after importing in Xcode:

![Alt text](http://i.imgur.com/DbFd38W.png "")


### 2. Import Facebook frameworks

###### 2.1. In folder CuuChauSDK extracted before, copy the children folder named Frameworks to your project folder as below:

![Alt text](http://i.imgur.com/pVVaWJB.png "")

###### 2.2. Drag Frameworks folder in to your project in Xcode and choose options as step 6.1, and the result like that:
 
![Alt text](http://i.imgur.com/Ea2poqL.png "")

### 3. Import SystemConfiguration.framework

###### 3.1. Click on the DemoSDK project in the project navigator, then select the DemoSDK target. Switch to the Build Phases tab. Click on children tab Link Binary With Libraries and click on the + button, as shown below:
 
![Alt text](http://i.imgur.com/LXuBNpt.png "")

###### 3.2. A popup is shown and search SystemConfiguration.framework, click on SystemConfiguration.framework and click on the Add button:

![Alt text](http://i.imgur.com/IMBWIQJ.png "")

### 4. Config project properties

###### 4.1. Click on the DemoSDK project in the project navigator, then select the DemoSDK target. Switch to the Info tab. Click on children tab Custom iOS Target Properties. Right-mouse click on any properties and choose Add Row to add those important properties:

![Alt text](http://i.imgur.com/0q4Y7E6.png "")

###### 4.2. Add game_code property:
- Key: game_code
- Type: String
- Value: your_game_code

![Alt text](http://i.imgur.com/BRNLOjY.png "")

###### 4.3. Add FacebookAppID property:
- Key: FacebookAppID
- Type: String
- Value: 1527768420832101
 
![Alt text](http://i.imgur.com/A2QZZrM.png "")

###### 4.4. Add FacebookDisplayName property:
- Key: FacebookDisplayName
- Type: String
- Value: 9chau

![Alt text](http://i.imgur.com/nO4jleY.png "")



### 5. Config URL types

###### 5.1. Click on the DemoSDK project in the project navigator, then select the DemoSDK target. Switch to the Info tab. Click on children tab URL Types and click on the + button:

![Alt text](http://i.imgur.com/nO4jleY.png "")

###### 5.2. Assign URL Schemes equals fb1527768420832101 as below:

![Alt text](http://i.imgur.com/d26cIAL.png "")

### 6. Prepare for using CuuChauSDK


###### 6.1. Open file AppDelegate.m

###### 6.2. Import Eway.h header: 

```
	#import "EWay.h"
```
###### 6.3. Replace content in function:
```
	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {} 
```

with the statement:

```
	return [EWay application:application didFinishLaunchingWithOptions:launchOptions];
```

###### 6.4. Replace content in function:

```
	- (void)applicationDidBecomeActive:(UIApplication *)application {} 
```
with the statement:

```
	return [EWay activeApp];
```

###### 6.5. Add new function:

```
	- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
	    return [EWay application:application
	                     openURL:url
	           sourceApplication:sourceApplication
	                  annotation:annotation];
	}
```

### 7. Add authentication function

###### 7.1. Import the header file Eway.h:

```
	#import "EWay.h"
```

###### 7.2. To show authentication function, use this script:

```
	[EWay authorizeWithCompleted:^(EWayUser *user, NSError *error) {
	if(!error) {
	     		//if success, your code here      
	     	} else {
	           //if error, your code here
	  	}
	}];
```

### 8. Add payment function

###### 8.1. Import the header file Eway.h:

```
	#import "EWay.h"
```

###### 8.2. To show payment function, use this script to payment button:

```
	[EWay showRechargePanelWithGameOrder:@"test" andCompletedBlock:^(NSError *error) {
		//if error, your processing code here        
	}];
```

Note: showRechargePanelWithGameOrder is json string and optional.



### 9. Add profile function

###### 9.1. Import the header file Eway.h:

```
	#import "EWay.h"
```

###### 9.2. In file .m that you want to add profile button, at tag @interface implements EwayDelegate, example: 

```
	@interface ViewController ()<EWayDelegate>

	@end
```

###### 9.3. To show profile, use this script:

```
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
