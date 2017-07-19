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

* Download the SDK from <a href="https://cdn.worldcoo.com/download/sdk/ios/Worldcoo-ios-latest.zip">here</a> and decompress the .zip file
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


# 3 - Initialization

These are the common public properties and methods, both for API integration and Widget injection

## 3.1 - static sharedInstance

_Return the instance of the SDK._

**return**

* Worldcoo’s SDK instance


## 3.2 - func initSDK(productionMode: Bool)

_Initializes the SDK_

**params**

* _productionMode_ : True if production mode or false if using the REST API in sandbox mode

**return**

* True on success
* False if some errors occurs loading API keys
 
 
## 3.3 - var productionMode
 
_Returns Donor API mode_

**return**

* True if production mode or false if using the REST API in sandbox mode



# 4 - API integration

> API integration example

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
```

These are the public methods provided by the SDK to interact with the Worldcoo Donation API.


## 4.1 - func getAPIClient(listener: DonorListener)

_Returns a client to easily interact with the Donor web API_

**params**

* _listener_ : Donorlistener instance

**return**

* DonorAPIClient instance


## 4.2 - func getAvailableNGOs(offset: Int, limit: Int)

> Response example

```swift
[
    {
        "id": "8720fdd5-1b10-a4c8-a614",
        "name": "NGO Name Example 1",
        "url": "http://www.examplengo1.com",
        "description": "This is an example of NGO description."
        "logos": {
            "S": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/logo.1479742005411.S.jpg",
            "M": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/logo.1479742005411.M.jpg",
            "L": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/logo.1479742005411.L.jpg"
        },
        "media": {},
    },
    {
        "id": "6582d7k1-7d61-99dc-b213",
        "name": "NGO Name Example 2",
        "url": "http://www.examplengo2.com",
        "description": "This is an example of NGO description."
        "logos": {
            "S": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/logo.1479742005411.S.jpg",
            "M": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/logo.1479742005411.M.jpg",
            "L": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/logo.1479742005411.L.jpg"
        },
        "media": {},
    },
    ...
]
```

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


## 4.3 - func getAvailableCampaigns(ngoId: String, offset: Int, limit: Int)

> Response example

```swift
[
    {
        "id": "a9fb530d-6270-0cc7-e8a8",
        "alias": "Demo campaign",
        "ngo_id": "2454de2c-cb31-44e5-83bd",
        "status": "active",
        "location": {
            "country": "ESP",
            "name": "Barcelona"
        },
        "categories": ["water_and_energy", "hunger_and_food_security"],
        "currency": "EUR",
        "collection_objective": {
            "total": 500,
            "distribution": {
                "materials": 100,
                "team": 100,
                "transport": 50,
                "others": 200,
                "providers": 50
            }
        },
        "beneficiaries": {
            "direct": 400,
            "indirect": 1000
        },
        "starting_date": 1464277166,
        "ending_date": null,
        "media": {
            "eaba63b5-d7be-4de8-a53e-283c9104a9a6": {
                "S": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.S.jpg",
                "L": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.L.jpg",
                "M": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.M.jpg",
                "XL": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.XL.jpg"
            },
            "89bfea45-5704-4e4b-9aab-b80c3ac1fa9c": {
                "S": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.S.jpg",
                "L": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.L.jpg",
                "M": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.M.jpg",
                "XL": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.XL.jpg"
            }
        },
        "counters": {
            "donors": 467,
            "donated": 5035
        },
        "texts": {
            "name": "NGO Campaign 1",
            "name_short": "Cmpgn 1",
            "resume": "This is an example of campaign description.",
            "resume_short": "CampaignDescription",
            "activities": "",
            "objectives": "",
            "benefits": "",
            "call_to_action": "",
            "worldcoo_url": ""
        }
    },
    ...
]
```


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


## 4.4 - func getCampaignDetails(ngoId: String, campaignId: String)

> Response example

```swift
{
    "id": "a9fb530d-6270-0cc7-e8a8",
    "alias": "Demo campaign",
    "ngo_id": "2454de2c-cb31-44e5-83bd",
    "status": "active",
    "location": {
        "country": "ESP",
        "name": "Barcelona"
    },
    "categories": ["water_and_energy", "hunger_and_food_security"],
    "currency": "EUR",
    "collection_objective": {
        "total": 500,
        "distribution": {
            "materials": 100,
            "team": 100,
            "transport": 50,
            "others": 200,
            "providers": 50
        }
    },
    "beneficiaries": {
        "direct": 400,
        "indirect": 1000
    },
    "starting_date": 1464277166,
    "ending_date": null,
    "media": {
        "eaba63b5-d7be-4de8-a53e-283c9104a9a6": {
            "S": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.S.jpg",
            "L": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.L.jpg",
            "M": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.M.jpg",
            "XL": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.XL.jpg"
        },
        "89bfea45-5704-4e4b-9aab-b80c3ac1fa9c": {
            "S": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.S.jpg",
            "L": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.L.jpg",
            "M": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.M.jpg",
            "XL": "https://cdn.worldcoo.com/ngos/ee80cf92-d49b-4b7c-9949-9946750ec451/campaigns/f6425839-1e6f-4dd2-8d4d-593d61f5437f/media/eaba63b5-d7be-4de8-a53e-283c9104a9a6.1479739685956.XL.jpg"
        }
    },
    "counters": {
        "donors": 467,
        "donated": 5035
    },
    "texts": {
        "name": "NGO Campaign 1",
        "name_short": "Cmpgn 1",
        "resume": "This is an example of campaign description.",
        "resume_short": "CampaignDescription",
        "activities": "",
        "objectives": "",
        "benefits": "",
        "call_to_action": "",
        "worldcoo_url": ""
    }
}
```


<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#get-campaign-details" target="_blank">Get campaign details</a> Worldcoo API endpoint.
</aside>

_Gets campaign details_

**params**

* _ngo_id_ : NGO identifier
* _campaign_id_ : Campaign identifier

**campaignDetail(campaign: Campaign)**

* _campaign_ : Campaign information

**onError(error: ErrorBody, type: RequestType)**

* _error_ : Error code, type and message
* _type_ : CAMPAIGN_DETAILS



## 4.5 - func addDonation(donation: Donation)

> Response example

```swift
{
    "id": "17a10455-120d-e767-0206",
    "amount": 5,
    "currency": "EUR",
    "order_code": "9638467",
    "campaign_counters": {
        "total_donated": 60,
        "target": 50000
    }
}
```

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


## 4.6 - func cancelDonation(donationId: String)

> Response example

```swift
{
    "id": "17a10455-120d-e767-0206",
    "amount": 5,
    "currency": "EUR",
    "order_code": "9638467",
    "campaign_counters": {
        "total_donated": 60,
        "target": 50000
    }
}
```

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


# 5 - Widget injection

> Widget Workflow example

```swift
// Construct and create the widget object
var sdk: WorldcooSDK = Worldcoo.getInstance sdk.initSdk(productionMode: false)

// WARNING! This needs to be done on viewDidAppear function to work correctly
var injector = sdk.getInjector(container: container!, widget: widget!, listener: self)

// In your "cart" view you can inject the first step widget to let the user decide if he wants to donate
injector?.initialStep()

// The user continues the checkout process
...
// The user pays and the checkout process arrives to the "thanks" step

// In your "thanks" view you can inject the confirmation step widget.
injector?.confirmation()

// If the user decided to donate, the donation will be sent to Worldcoo system and a thanks message will be shown
// Otherwise, no donation will be sent and nothing will be shown
```

This is the way you can use the Worldcoo Widget inside your mobile app.

Using the Worldcoo Widget is a two-steps process:

* First of them is to inject the Initial Widget [_initialStep()_ method] where the user decides if he is going to donate or not and also the donation amount

* Last step consists on injecting the Confirmation Widget [_confirmation()_ method], where the user choice is evaluated. So, depending on what the user selected on the first step, a donation will be set on out system or not.

For sure, you'll need to know what the user choice is, so through the InjectorListener you can listen to the _change_ event while the Initial Widget is shown to act in consequence. For example, you'll need to add the selected donation amount to the cart total amount or maybe you want set a "withDonation" flag to your order.

<aside class="notice">
    You can read more about the Worldcoo Widget workflow <a href="http://docs.worldcoo.com/widget/v3/#2-step-integration" target="_blank">here</a>
</aside> 

## 5.1 - func getInjector(container: UIView, widget: Widget, listener: InjectorListener)

_Returns a widget injector instance_

**params**

* _UIView_ : UIView container where the widget will be charge 
* _widget_ : Widget object with information about the donor and donation
* _listener_ : Injector listener

**return**

* A WidgetInjector instance


## 5.2 - func initialStep()

_Injects the widget first step on the given webview._


## 5.3 - func confirmation()

_Injects the widget second step on the given webview._
