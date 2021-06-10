# LoginAuth Framework for iOS

![](https://img.shields.io/badge/version-1.0-blue) ![](https://img.shields.io/badge/release-blueviolet) ![](https://img.shields.io/badge/easyinuse-release) ![](https://img.shields.io/badge/AppStore-ready-important)

## Description

LoginAuth Framework is created by DATA MOTION PTE. LTD. and allows you to integrate with our UserService API in a few steps.

LoginAuth Framework has three main functions:
1. Creating `deviceToken` for each new Telematics SDK user.
2. Refeshing the `jwToken` when it is expired.
3. Geting `jwToken` for existing SDK User.


## Credentials

Before you start using the framework, make sure you registered a company account in the [Datahub](https://userdatahub.com/) and obtained `InstanceId` and`InstanceKey`. If you are new, please refer to the [documentation](doc.telematicssdk.com) and register your company account in Datahub. [Sing Up](https://userdatahub.com/user/registration)


## LoginAuth Framework setup

To integrate the framework, you need to perform a few simple steps:

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

`deviceToken` - is the main individual SDK user identifier for your app. this identifier is used as a key across all our services.

`jwToken` - or JSON Web Token (JWT) is the main UserService API key, that allows you to get user individual statistics and user scorings by UserService APIs calls.

`refreshToken` - is a secret key that allows you to refresh the `jwToken` when it expires.

For your convenience, these three actions are implemented in our LoginAuth Framework in one line of code as it is.

## Methods
### Create DeviceToken

Each SDK user has to have a `Devicetoken` and be associated with the app users. To create `DeviceToken` please use the method below. To complete a call, you are required to provide `instanceId` & `instanceKey`. If you still have quiestions on how to obtain the credentails, please refer to the [documentation](https://dev.telematicssdk.com/docs/datahub#user-service-credentials)

Objective-C

     [[LoginAuthCore sharedManager] createDeviceTokenForUserWithInstanceId:@"instanceId"
                                                               instanceKey:@"instanceKey"
                                                                    result:^(NSString *deviceToken, NSString *jwToken, NSString *refreshToken) {
        NSLog(@"LoginAuthResponce deviceToken %@", deviceToken);
        NSLog(@"LoginAuthResponce jwToken %@", jwToken);
        NSLog(@"LoginAuthResponce refreshToken %@", refreshToken);
    }];

Swift

     LoginAuthCore.sharedManager()?.createDeviceTokenForUser(withInstanceId: "instanceId",
                                                                instanceKey: "instanceKey",
                                                                result: { (deviceToken, jwToken, refreshToken) in
            print("Success Create User")
    })

Once user is registered, you will receive the user credentails. make sure you pass the `Devicetoken` to your server and store it against a user profile, then pass it to your App - this is the main user detials that you will use for our services.


### Refresh JWT

Each `JWTtoken` has a limmited lifetime and in a certain period of time it is expired. As a result, when you call our API using invalid `JWTtoken` you will receive an Error `Unauthorized 401`.
**Error 401** indicates that the user's `JWTtoken` has been expired. If so, as the first step, you have to update the `JWToken`.

To update the `JWTtoken`, you are required to provide the latest `JWTtoken` & `refreshToken` to the method below.

Objective-C

    [[LoginAuthCore sharedManager] refreshJWTokenForUserWith:@"jwToken"
                                                refreshToken:@"refreshToken"
                                                      result:^(NSString *newJWToken, NSString *newRefreshToken) {

        NSLog(@"NEW jwToken %@", newJWToken);
        NSLog(@"NEW refreshToken %@", newRefreshToken);
    }];

Swift

    LoginAuthCore.sharedManager()?.refreshJWTokenForUser(with: "jwToken",
                                                             refreshToken: "refreshToken",
                                                             result: { (newJWToken, newRefreshToken) in
            print("Success Refresh jwToken & refreshToken")
    })
In response you will receive new `JWTtokens`.


### Get JWT for existing SDK users

During the app usage, there may be several scenarios when the app loses `JWTtoken`, for example if the a user changes a smartphone or logs out. BTW, that is a reason why we strongly recommend you to store the `deviceToken` on your backend side. `deviceToken` cannot be restored if it is lost!

We provide you with a simple re-authorization, a method that you can use to get a valid `JWTtoken` for a particular user throught providing `DeviceToken`
To use this mehod, you need `deviceToken`, `instanceId`, and `instanceKey` of which group the user belongs. in this case, `Devicetoken` works as a login, `instancekey` as a password. Then you can re-login the user and get a valid `JWTtoken` & `refreshToken`.

Objective-C

    [[LoginAuthCore sharedManager] getJWTokenForUserWithDeviceToken:@"deviceToken"
                                                         instanceId:@"instanceId"
                                                        instanceKey:@"instanceKey"
                                                             result:^(NSString *jwToken, NSString *refreshToken) {
        NSLog(@"NEW JWT by DEVICETOKEN %@", jwToken);
        NSLog(@"NEW REFRESHTOKEN by DEVICETOKEN %@", refreshToken);
    }];

Swift

    LoginAuthCore.sharedManager()?.getJWTokenForUser(withDeviceToken: "deviceToken",
                                                         instanceId: "instanceId",
                                                         instanceKey: "instanceKey",
                                                         result: { (jwToken, refreshToken) in
            print("Success Getting New jwToken & refreshToken by DeviceToken")
    })

In response, you will receive a new `jwToken` and `refreshToken`.

Happy coding!


## Links

[Official product Web-page](https://telematicssdk.com/)

[Official API services web-page](https://www.telematicssdk.com/api-services/)

[Official SDK and API references](https://www.telematicssdk.com/api-services/)

[Official ZenRoad web-page](https://www.telematicssdk.com/telematics-app/)

[Official ZenRoad app for iOS](https://apps.apple.com/jo/app/zenroad/id1563218393)

[Official ZenRoad app for Android](https://play.google.com/store/apps/details?id=com.telematicssdk.zenroad&hl=en&gl=US)

[Official ZenRoad app for Huawei](https://appgallery.huawei.com/#/app/C104163115)

###### Copyright © 2020-2021 DATA MOTION PTE. LTD. All rights reserved.

