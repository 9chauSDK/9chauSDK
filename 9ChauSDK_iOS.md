# How to integrate IOS SDK

=======ẢNH Ở ĐÂY=================
http://imgur.com/a/KtJ9w

6.1. Import 9chauSDK

1.  Extract file CuuChauSDK.zip, we get a folder named CuuChauSDK. Copy two children folders named include and lib to your project folder as below:
 

2. Drag that folders into your project in Xcode and choose options like that:
 
and the result after importing in Xcode:
 

6.2. Import Facebook frameworks:

1. In folder CuuChauSDK extracted before, copy the children folder named Frameworks to your project folder as below:
 

2. Drag Frameworks folder in to your project in Xcode and choose options as step 6.1, and the result like that:
 

6.3. Import SystemConfiguration.framework

1. Click on the DemoSDK project in the project navigator, then select the DemoSDK target. Switch to the Build Phases tab. Click on children tab Link Binary With Libraries and click on the + button, as shown below:
 

2. A popup is shown and search SystemConfiguration.framework, click on SystemConfiguration.framework and click on the Add button:
 

6.4. Config project properties:
1. Click on the DemoSDK project in the project navigator, then select the DemoSDK target. Switch to the Info tab. Click on children tab Custom iOS Target Properties. Right-mouse click on any properties and choose Add Row to add those important properties:
 

2. Add game_code property:
- Key: game_code
- Type: String
- Value: your_game_code

 

3. Add FacebookAppID property:
- Key: FacebookAppID
- Type: String
- Value: 1527768420832101
 

4. Add FacebookDisplayName property:
- Key: FacebookDisplayName
- Type: String
- Value: 9chau
 

6.5. Config URL types:

1. Click on the DemoSDK project in the project navigator, then select the DemoSDK target. Switch to the Info tab. Click on children tab URL Types and click on the + button:
 

2. Assign URL Schemes equals fb1527768420832101 as below:
  

6.6. Prepare for using CuuChauSDK:
1. Open file AppDelegate.m
2. Import Eway.h header: 
#import "EWay.h"
3. Replace content in function:
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {} 
with the statement:

return [EWay application:application didFinishLaunchingWithOptions:launchOptions];

3. Replace content in function:
- (void)applicationDidBecomeActive:(UIApplication *)application {} 
with the statement:

return [EWay activeApp];

4. Add new function:
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [EWay application:application
                     openURL:url
           sourceApplication:sourceApplication
                  annotation:annotation];
}

6.7. Add authentication function:
1. Import the header file Eway.h:
#import "EWay.h"
2. To show authentication function, use this script:
[EWay authorizeWithCompleted:^(EWayUser *user, NSError *error) {
if(!error) {
     		//if success, your code here      
     	} else {
           //if error, your code here
  	}
}];

6.8. Add payment function:
1. Import the header file Eway.h:
#import "EWay.h"
2. To show payment function, use this script to payment button:
[EWay showRechargePanelWithGameOrder:@"test" andCompletedBlock:^(NSError *error) {
	//if error, your processing code here        
}];

Note: showRechargePanelWithGameOrder is json string and optional.

6.9. Add profile function:
1. Import the header file Eway.h:
#import "EWay.h"
2. In file .m that you want to add profile button, at tag @interface implements EwayDelegate, example: 
@interface ViewController ()<EWayDelegate>

@end
3. To show profile, use this script:
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
