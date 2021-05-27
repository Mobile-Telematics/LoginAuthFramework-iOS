# LoginAuth Framework for iOS

![](https://img.shields.io/badge/version-1.0-blue) ![](https://img.shields.io/badge/release-blueviolet) ![](https://img.shields.io/badge/easyinuse-release) ![](https://img.shields.io/badge/AppStore-ready-important)

## Description

LoginAuth Framework is created by DATA MOTION PTE. LTD. and allows you to integrate with our UserService API in a few steps.

LoginAuth Framework has three main functions:
1. Creating `deviceToken` for each new Telematics SDK user.
2. Refeshing the `jwToken` when it is expired.
3. Geting `jwToken` for existing SDK User.


## Credentials

Before you start using the framework, make sure you registered a company accouunt in the [Datahub]() and obtained `InstanceId` and`InstanceKey`. If you are new, please refer to the [documentation](doc.telematicssdk.comm) and register your company account in [Datahub](https://userdatahub.com/user/registration)


## LoginAuth Framework setup

To integrate, you need to perform a few simple steps:

Step 1: Download the `LoginAuth.xcframework` library from this repository.

Step 2: Copy `LoginAuth.xcframework` to your iOS project folder where the rest of your project files are located.

Step 3: Open your project in xCode.

Step 4: Go to the first General tab of your project, select your Target.

Step 5: Scroll down to the "Frameworks, Libraries and Embeddeed Content" section.

Step 6: Drag&Drop`LoginAuth.xcframework` from Finder project folder to this section, OR click the "Plus" button below, and select "Add Other" and manually specify the path to the `LoginAuth.xcframework` location.

![](https://github.com/Mobile-Telematics/LoginAuthFramework-iOS/raw/master/images/al.png)

Step 7: Add in header AppDelegate.h (or any file need for you) of your project this line `#import <LoginAuth/LoginAuth.h>`

Step 8: Enjoy!


## Basic concepts

`deviceToken` - is the main individual SDK user identifier for your app to transfer user trips to the our SDK Platform. All user trips are recorded and accessible only using the deviceToken.

`jwToken` - or JSON Web Token (JWT) is the main UserService API key, that allows you to get user individual statistics and user scorings by UserService APIs calls.

`refreshToken` - is a secret key that allows you to refresh the `jwToken` when it expires.

For your convenience, these three actions are implemented in our LoginAuth Framework in one line of code as it is.


## How to use - Create DeviceToken

Just use this method and pass `instanceId` & `instanceKey` in it.

     [[LoginAuthCore sharedManager] createDeviceTokenForUserWithInstanceId:@"instanceId"
                                                               instanceKey:@"instanceKey"
                                                                    result:^(NSString *deviceToken, NSString *jwToken, NSString *refreshToken) {
        NSLog(@"LoginAuthResponce deviceToken %@", deviceToken);
        NSLog(@"LoginAuthResponce jwToken %@", jwToken);
        NSLog(@"LoginAuthResponce refreshToken %@", refreshToken);
    }];

User has been created. Congratulations! 

Save the received keys in your App - this is the main user data.


## How to use - Refresh JWT

When you request individual statistics or scorings for user in our UserService API, you can get an `Unauthorized 401` response.
Error 401 indicates that the user's token has expired. In this case, the first step is to update the jwToken.

To update the jwToken, you need to pass the previously saved "old" `jwToken` & `refreshToken` in the method below.

    [[LoginAuthCore sharedManager] refreshJWTokenForUserWith:@"jwToken"
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

    [[LoginAuthCore sharedManager] getJWTokenForUserWithDeviceToken:@"deviceToken"
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
