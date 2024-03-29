
StreamSDK Authentication Module
=====================
The Authentication SDK provides simple access to StreamAMG's Authentication and Key Session services.

It keeps track of the current user's session, allowing automatic login for know validated users, and allows instant access to Key Sessions for given Entry IDs

Quick Start Guide
======

Cocoapods implementation
=====

Currently, the only supported way of installing the StreamAMG SDK is via Cocoapods.

The Core module is automatically installed as a pod dependency for all pods that support it

If Cocoapods is not installed on your Mac, follow the instructions [here](https://guides.cocoapods.org/using/getting-started.html)

If Cocoapods is not initialted for your project, open a terminal window, navigate to the root directory of your project and enter the following command:

```
pod init
```

In the Podfile file that has been created, add the following line after the '# Pods for (your project) line:

```
pod 'StreamAMGSDK/Authentication'
```

Now, again from the rood directory of your project, in a terminal, enter the following command:

```
pod install (or... pod update to also refresh any pods you already have)
```

The pod will be installed and available to use when your project is opened using the newly created (projectname).xcworkspace file, this should always be used instead of the xcodeproj file in a pods project.

Please use the same version for all modules to prevent dependency errors.

API Overview
============

##Accesing the SDK

To access the SDK, you should import it in any swift file that requires it:

```
import StreamAMGSDK
```

##Setting up Authentication

The authentication SDK should be accessed via it's singleton instance

```
var auth = AuthenticationSDK.instance
```

It is required that a valid authentication URL is passed to the SDK before it is usable:

```
auth.initialiseWithURL("https:validURL.test.com")
```

It is also possible to pass other URL parameters to the SDK here:

```
auth.initialiseWithURL("https:validURL.test.com", params:"lang=en&otherParam=otherValue")
```

Logging In
========

To authenticate with the selected StreamAMG Authentication API, simply pass an email and password to the login function, with a completion block

```
auth.login(email: "User Email", password: "User Password"){ (result: Result<StreamAMGUserModel, StreamAMGError>) in
    switch result {
    case .success(let userModel):
        // User is successfully logged in here
    case .failure(let error):
        // Login failed
    }
}
```

Once a user has logged in successfully, their details are securely stored on the device's KeyChain. These details can be accessed to populate login fields if required:

```
if let details = auth.securelyRetrieveEmailAndPass(){
    emailTextField.text = details.email
    passwordTextField.text = details.password
}
```

This returns an optional tuple (email: String, password: String)?

Alternatively, it is also possible to automatically login a previously verified user as long as they have not since logged out:

```
auth.loginSilent{ (result: Result<StreamAMGUserModel, StreamAMGError>) in
    switch result {
    case .success(let userModel):
        // User is successfully logged in here
    case .failure(let error):
        // Login failed
    }
}
```

Logging Out
=========

To end a user's session and remove the user's credentials from the Keychain, you can log the user out:

```
auth.logout { (result: Result<SAResult, StreamAMGError>) in
    switch result {
    case .success(_):
        // User is successfully logged out here
    case .failure(let error):
        // Logout failed
    }
}
```

Requesting Key Session Token
=======================

If the Authentication API is providing Key Session Tokens, these can also be requested via the SDK:

```
auth.getKS(entryID: "0_validEntryID") { (result: Result<(SAKSResult, String), StreamAMGError>) in
    switch result {
    case .success(let response):
        // response.1 is the valid KS
    case .failure(let error):
        // error includes the reason the Key Session is not provided
    }
}
```
Using custom SSO
=======================
Authentication SDK supports custom SSO implementation that are correctly configured with CloudPay and are able to generate a valid use JWT token.

## Start the session with custom SSO

In order to communicate with CloudPay the app needs to start the session with the users token generated. This is an optional step and required only if you are not using cloud pay login but you want to start a user session to to log the sessions and so get CloudPay check the concurrency.


```
auth.startSession(token: tokens.idToken) { error in
  if error != nil {
   // Handle error
  } else {
   // Session sucessfully created
  }                             
}
```
## Update user summary
The update user summary function allows a user to update their user details. Please be aware that the API cannot edit the user's email.

This function needs to be utilised after a new user is registered and every time the user wants to update the metadata.

```
auth.updateUserSummaryWithUserToken(token: tokens.idToken, firstName: "Name", lastName: "Surname") {(result: Result<UserSummaryResponse, StreamAMGError>) in
  switch result {
   case .success(let data):
    // User data sucessfully updated.
   case .failure(let error):
    // Update user summary failed.                                          
  }
}
```
## Retrieve the User's Summary

Returns an account summary of the Authenticated User.

```
auth.getUserSummary(token: tokens.idToken) { (result: Result<UserSummaryResponse, StreamAMGError>) in
 switch result {
  case .success(let data):
  // Get user data success
  case .failure(let error):
  // Get user data failed
 }
}
```

Change Log:
===========

All notable changes to this project will be documented [here](Changelog.md)
