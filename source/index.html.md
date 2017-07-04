---
title: WorldCoo iOS SDK

language_tabs:
  - Swift

toc_footers:
  - <a href='https://github.com/WorldCoo/docs-slate-sdk-ios' target='_blank'>See on Github</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# iOS SDK overview

The Worldcoo iOS SDK allows to this Operating System developers to interact with the <a href="http://docs.worldcoo.com/api/v3" target="_blank">Worldcoo Donation API</a> in a native way through a series of public methods and, also, to inject the <a href="http://docs.worldcoo.com/widget/v3" target="_blank">Worldcoo Widget</a> in an easier way than developing a custom integration.


# 1 - Installation

Follow these steps to include the Worldcoo SDK inside your app:

* Download the SDK from <a href="https://cdn.worldcoo.com/download/sdk/ios/Worldcoo-ios-1.0.zip">here</a> and decompress the .zip file
* In Xcode, go to General in the project settings
* Create a folder /lib and include the framework on it
* Add the framework as embedded framework
* Drag and drop the framework to the linked framework and libraries
* Mark the linked framewok as Required



# 2 - Authentication

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>API_KEY</key>
        <string><!-- API KEY HERE --></string>
        <key>WIDGET_ID</key>
        <string><!-- WIDGET ID HERE --></string>
    </dict>
</plist>
```

In order to have access to the Worldcoo’s API and the widget generator a pair of keys are provided to the integrators. Those keys should be added by the developer into app’s code in a proper way to initialize the SDK successfully (keys API_KEY and WIDGET_ID needs to be declared without any modifications).

Since resources are declared using XML files, some values need to be declared using ASCII (‘-’ character should be &#8211)

Keys should be included in a property list named ApiKeys.plist.

If everything has been declared in a proper way, the SDK method init(..) will load the keys automatically.


# 3 - Usage

## 3.1 - Common

These are the common public properties and methods, both for API integration and Widget injection

### 3.1.1 - static sharedInstance

_Return the instance of the SDK._

**return**

* Worldcoo’s SDK instance


### 3.1.2 - func initSDK(productionMode: Bool)

_Initializes the SDK_

**params**

* _productionMode_ : True if production mode or false if using the REST API in sandbox mode

**return**

* True on success
* False if some errors occurs loading API keys
 
 
### 3.1.3 - var productionMode
 
_Returns Donor API mode_

**return**

* True if production mode or false if using the REST API in sandbox mode



## 3.2 - API integration

These are the public methods provided by the SDK to interact with the Worldcoo Donation API.


### 3.2.1 - func getAPIClient(listener: DonorListener)

_Returns a client to easily interact with the Donor web API_

**params**

* _listener_ : Donorlistener instance

**return**

* DonorAPIClient instance


### 3.2.2 - func getAvailableNGOs(offset: Int, limit: Int)

<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#get-available-ngos" target="_blank">Get Available NGOs</a> Worldcoo API endpoint.
</aside>

_Gets available NGOs_

**params**

* _offset_ : The amount of items to ignore. _(optional value, default=0)_
* _limit_ : The maximum number of items to return. _(optional value, default=20)_

**return**

* availableNGOs(ngos: [NGO])

**onError(error: ErrorBody, type: RequestType)**

* _error_ : Error code, type and message
* _type_ : GET_NGOS


### 3.2.3 - func getAvailableCampaigns(ngoId: String, offset: Int, limit: Int)

<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#get-available-campaigns" target="_blank">Get available campaigns</a> Worldcoo API endpoint.
</aside>

_Gets available campaign from an NGO._

**params**

* _ngoId_ : NGO identifier
* _offset_ : The amount of items to ignore. _(optional value, default=0)_
* _limit_ : The maximum number of items to return. _(optional value, default=20)_

**return**

* availableCampaigns(campaigns: [Campaign], total: Int)

**onError(error: ErrorBody, type: RequestType)**

* _error_ : Error code, type and message
* _type_ : GET_CAMPAIGNS


### 3.2.4 - func addDonation(donation: Donation)

<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#add-donation" target="_blank">Add donation</a> Worldcoo API endpoint.
</aside>

_Adds a donation_

**params**

* _donation_ : Donation information

**onDonationAdded(donation: DonationResponse)**

* _donation_ : Donation information

**onError(error: ErrorBody, type: RequestType)**

* _error_ : Error code, type and message
* _type_ : ADD_DONATION


### 3.2.4 - func cancelDonation(donationId: String)

<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#cancel-donation" target="_blank">Cancel donation</a> Worldcoo API endpoint.
</aside>

_Cancels a donation_

**params**

* _donationId_ : Donation identifier

**onDonationCancel(donation: DonationResponse)**

* _donation_ : Donation information

**onError(error: ErrorBody, type: RequestType)**

* _error_ : Error code, type and message
* _type_ : CANCEL_DONATION


## 3.3 - Widget injection

### 3.3.1 - func getInjector(container: UIView, widget: Widget, listener: InjectorListener)

_Returns a widget injector instance_

**params**

* _UIView_ : UIView container where the widget will be charge 
* _widget_ : Widget object with information about the donor and donation
* _listener_ : Injector listener

**return**

* A WidgetInjector instance


### 3.3.2 - func initialStep()

_Injects the widget first step on the given webview._


### 3.3.3 - func confirmation()

_Injects the widget second step on the given webview._


## 3.4 - Example

```swift
// SDK initialization
var sdk: WorldcooSDK = Worldcoo.getInstance sdk.initSdk(productionMode: false)

// Donor API Client
sdk.getAPIClient().getAvailableNGOs(listener: self) // Being call from a controller that implements NativeListener interface

// This will be received from callback:
onAvailableNGOs(ngos: [NGO])

// If there are errors
onError(error: ErrorBody, request: RequestType)

// Same way to proceed with all API methods

// Injector
// Construct and create the widget object
// WARNING! This needs to be done on viewDidAppear function to work correctly var injector = sdk.getInjector(
container: container!, widget: widget!, listener: self)
injector?. injectWidgetStep1() injector?. injectWidgetStep2()
```