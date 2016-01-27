# AvocarrotMoPubNativeAdapter

This repo is a native Avocarrot adapter for MoPub. If you like your UICollectionView Placer from MoPub, or already have MoPub integrated and don't want to code in yet another framework - this is the repo for you. This will allow you to use Native ads from Avocarrot, through MoPub, in your app.

## Prerequisites
To use this adapter you will need to do a few thing:
- Sign up with MoPub if you have not already done so. Do that here: https://app.mopub.com/account/register/
- Sign up with Avocarrot. Do that here: https://app.avocarrot.com/#/register
- Add CocoaPods to your project, if it is not already using it. Do that here: https://cocoapods.org/

Now that that is takin care of, you will need to follow the Avocarrot instruction on how to add their framework to your project. Only add the actual Framework file, don't do all the code integration parts of their tutorial. This repo will obviate the need for that part.

If you do not already have MoPub installed, then just use this cocoaPod and add it to your Pod file:
https://cocoapods.org/?q=Mopub

## Usage
Add the following files from this repo into your project (they are using ARC):
- AppNativeAdRenderer.h/.m
- AvocarrotNativeAdAdapter.h/.m
- AvocarrotNativeCustomEvent.h/.m

Configure and install the MoPub placer if you have not already done so. 

Next, you need to configure the MoPub Network settings for the Avocarrot ad network as so.
- Create a new Network named Avocarrot Native (or something similar)
- Set the custom event class to: AvocarrotNativeCustomEvent
- Set the custom event class data to:
```
{"apiKey":"\<your avocarrot api key>",
"placementId":"\<your avo carrot placement id>",
"sandbox":true}
```
Leave sandbox true for now until you have it all working. Then you can switch it from the server later and set the ads to live. Insert your actual Avocarrot api key and placementId from their portal for the placemnt that you have configured (in Avocarrot).

In the UIViewController where your UITableView or UICollectionView is (where you are dislaying native ads), you will want to modify (if you already have it there) or add the MoPub initializor so that it uses our AppNativeAdRenderer class.

```
// MoPub Initialization
    MPStaticNativeAdRendererSettings *settings = [[MPStaticNativeAdRendererSettings alloc] init];
    settings.renderingViewClass = [NativeAdCell class]; <-- Your native ad cell class
    settings.viewSizeHandler = ^(CGFloat maxWidth) {
        return [self collectionView:self.collectionView layout:self.collectionView.collectionViewLayout sizeForItemAtIndexPath:[NSIndexPath indexPathForItem:0 inSection:0]];
    };
    // MoPub renderer, here is where we insert our own renderer so that our class can be dynamically loaded
    MPNativeAdRendererConfiguration *config = [AppNativeAdRenderer rendererConfigurationWithRendererSettings:settings];
    _placer = [MPCollectionViewAdPlacer placerWithCollectionView:self.collectionView viewController:self rendererConfigurations:@[config]];
    _placer.delegate = self;
```

Run your app, and if all is configured properly you will see the Avocarrot test ads.

