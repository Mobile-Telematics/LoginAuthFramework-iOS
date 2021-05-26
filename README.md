# LoginAuthFramework for iOS

![](https://img.shields.io/badge/version-1.0-blue) ![](https://img.shields.io/badge/release-blueviolet) ![](https://img.shields.io/badge/easyinuse-release) ![](https://img.shields.io/badge/AppStore-ready-important)

## Description

LoginAuthFramework is created by DATA MOTION PTE. LTD. and allows you to integrate with our UserService API in a few steps.

LoginAuthFramework has three main functions:
1. Getting `deviceToken` for each user with one line of code.
2. Updating the JWToken when it expires.
3. Get a new JWToken using the previously received `deviceToken` of the user for login.


## Credentials

For commercial use, you need create sandbox account https://userdatahub.com/user/registration and get `InstanceId` and`InstanceKey` auth keys to work with our LoginAuthFramework.


## LoginAuthFramework setup

To integrate, you need to perform a few simple steps:

Step 1: Download the `LoginAuthFramework.xcframework` library from this repository.

Step 2: Copy `LoginAuthFramework.xcframework` to your iOS project folder where the rest of your project files are located.

Step 3: Open your project in xCode.

Step 4: Go to the first General tab of your project, select your Target.

Step 5: Scroll down to the "Frameworks, Libraries and Embeddeed Content" section.

Step 6: Drag&Drop`LoginAuthFramework.xcframework` from Finder project folder to this section, OR click the "Plus" button below, and select "Add Other" and manually specify the path to the `LoginAuthFramework.xcframework` location.

![](https://raw.githubusercontent.com/pavelbowie/UserServiceFramework-ios_test/main/images/us1.png)

Step 7: Add in header AppDelegate.h (or any file need for you) of your project this line `#import <LoginAuthFramework/LoginAuthFramework.h>`

Step 8: Enjoy!


## Basic concepts

`deviceToken` - is the main individual user identifier. All user trips are recorded and accessible only using the deviceToken.

`jwToken` - or JSON Web Token (JWT) is the main UserService API key, that allows you to get user individual statistics and user scorings.

`refreshToken` - is a secret key that allows you to refresh the jwToken when it expires.

For your convenience, these three actions are implemented in our LoginAuthFramework in one line of code as it is.


## How to use - Create Device Token

Just use this method and pass `instanceId` & `instanceKey` in it.

     [[LoginAuthFrameworkCore sharedManager] createDeviceTokenForUserWithInstanceId:@"instanceId"
                                                                        instanceKey:@"instanceKey"
                                                                             result:^(NSString *deviceToken, NSString *jwToken, NSString *refreshToken) {
        NSLog(@"LoginAuthFrameworkResponce deviceToken %@", deviceToken);
        NSLog(@"LoginAuthFrameworkResponce jwToken %@", jwToken);
        NSLog(@"LoginAuthFrameworkResponce refreshToken %@", refreshToken);
    }];

User has been created. Congratulations! 

Save the received keys in your App - this is the main user data.


## How to use - Refresh JWT

To update the jwToken, you need to pass the previously saved "old" jwToken & refreshToken in the method below.

    [[LoginAuthFrameworkCore sharedManager] refreshJWTokenForUserWith:@"jwToken"
                                                         refreshToken:@"refreshToken"
                                                               result:^(NSString *newJWToken, NSString *newRefreshToken) {

        NSLog(@"NEW jwToken %@", newJWToken);
        NSLog(@"NEW refreshToken %@", newRefreshToken);
    }];

In response you will receive new tokens. Good.


## How to use - Get JWT with help of DeviceToken

When using app, there are scenarios when the user (for example) deleted the app or logged out. We strongly require and warn you about the need to save the `deviceToken` in the your app or in your backend side. `deviceToken` cannot be restored if it is lost!

We provide you with simple re-authorization.
If you have a `deviceToken`, as well as `instanceId` and `instanceKey` received during registration, you can simply re-login the user to your app and receive a new jwToken & refreshToken for him.

    [[LoginAuthFrameworkCore sharedManager] getJWTokenForUserWithDeviceToken:@"deviceToken"
                                                                  instanceId:@"instanceId"
                                                                 instanceKey:@"instanceKey"
                                                                      result:^(NSString *jwToken, NSString *refreshToken) {
        NSLog(@"NEW JWT by DEVICETOKEN %@", jwToken);
        NSLog(@"NEW REFRESHTOKEN by DEVICETOKEN %@", refreshToken);
    }];

In response, you will receive new `jwToken` and `refreshToken`, which will help you request statistics and scorings for each user.

Happy coding!


## Links

[https://telematicssdk.com/](https://telematicssdk.com/)

[Official ZenRoad app for iOS](https://apps.apple.com/us/app/zenroad/id1432161345/)

[Official ZenRoad app for Android](https://play.google.com/store/apps/details?id=com.raxeltelematics.zenroad&hl=ru&gl=US)


###### Copyright © 2020-2021 DATA MOTION PTE. LTD. All rights reserved.

















