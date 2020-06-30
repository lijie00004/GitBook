[GoogleAd教程网站](https://developers.google.com/admob/android/banner)

---

## 添加 SDK

```java
dependencies {
    implementation 'com.google.android.gms:play-services-ads:19.1.0'
}
<uses-permission android:name="com.android.vending.BILLING"/>
<application
    <meta-data
        android:name="com.google.android.gms.ads.APPLICATION_ID"
        android:value="ca-app-pub-4139451859925820~3239337129"/>   
</application>
```

## 出错

```java
提示 android.support.customtabs.ICustomTabsCallback
    
Open gradle.properties and add the following 2 lines:
	android.useAndroidX=true
	android.enableJetifier=true
        
In build.gradle      
    dependencies {
        api 'androidx.multidex:multidex:2.0.1'
    }

Clean and Rebuild the project again
```



## Sample

* AppActivity.java 参考 FacebookAd

```java
package org.cocos2dx.javascript;

import android.util.Log;
import android.view.ViewGroup;
import android.widget.RelativeLayout;
import android.widget.Toast;

import androidx.annotation.NonNull;

import com.google.android.gms.ads.AdListener;
import com.google.android.gms.ads.AdRequest;
import com.google.android.gms.ads.AdSize;
import com.google.android.gms.ads.AdView;
import com.google.android.gms.ads.InterstitialAd;
import com.google.android.gms.ads.MobileAds;
import com.google.android.gms.ads.initialization.InitializationStatus;
import com.google.android.gms.ads.initialization.OnInitializationCompleteListener;
import com.google.android.gms.ads.rewarded.RewardItem;
import com.google.android.gms.ads.rewarded.RewardedAd;
import com.google.android.gms.ads.rewarded.RewardedAdCallback;
import com.google.android.gms.ads.rewarded.RewardedAdLoadCallback;

import org.cocos2dx.lib.Cocos2dxJavascriptJavaBridge;

public class GoogleAd {
    private final String TAG = "GoogleAd";
    private static final String[] rewarded_box_id = {"ca-app-pub-4139451859925820/9421602098", "ca-app-pub-4139451859925820/3620866106"};
    private static final String[] rewarded_home_id = {"ca-app-pub-4139451859925820/1186274456", "ca-app-pub-4139451859925820/6312292838"};
    private static final String[] rewarded_topUp_id = {"ca-app-pub-4139451859925820/9655506006", "ca-app-pub-4139451859925820/5406884201"};
    private static final String[] interstitial_vectory_id = {"ca-app-pub-4139451859925820/4128982623", "ca-app-pub-4139451859925820/5827941630"};
    private static final String[] interstitial_startGame_id = {"ca-app-pub-4139451859925820/1502819283", "ca-app-pub-4139451859925820/8064358205"};
    private static final String banner_id = "ca-app-pub-4139451859925820/2181476139";
    private static final String test_id = "ca-app-pub-3940256099942544/5224354917";
    private static final String test_id_new = "ca-app-pub-3940256099942544/1033173712";
    private static AppActivity self;
    protected RelativeLayout m_FrameLayout;
    public AdView mAdView;
    private static RewardedAd rewardedAd_box_0;
    private static RewardedAd rewardedAd_box_1;
    private static RewardedAd rewardedAd_home_0;
    private static RewardedAd rewardedAd_home_1;
    private static RewardedAd rewardedAd_topUp_0;
    private static RewardedAd rewardedAd_topUp_1;
    private static InterstitialAd interstitialAd_startGame_0;
    private static InterstitialAd interstitialAd_startGame_1;
    private static InterstitialAd interstitialAd_victory_0;
    private static InterstitialAd interstitialAd_victory_1;

    public GoogleAd(AppActivity appActivity, RelativeLayout mFrameLayout) {
        self = appActivity;
        m_FrameLayout = mFrameLayout;
        MobileAds.initialize(self, new OnInitializationCompleteListener() {
            @Override
            public void onInitializationComplete(InitializationStatus initializationStatus) {
                initRewardedAd();
                initInterstitialAd();
            }
        });
    }

    public void googleBannerAd() {
        mAdView = new AdView(self);
        mAdView.setAdSize(AdSize.SMART_BANNER);
        mAdView.setAdUnitId("ca-app-pub-3940256099942544/6300978111");
        RelativeLayout.LayoutParams params = new RelativeLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);
        params.addRule(RelativeLayout.ALIGN_PARENT_BOTTOM);
        params.addRule(RelativeLayout.CENTER_HORIZONTAL);
        m_FrameLayout.addView(mAdView, params);

        mAdView.setAdListener(new AdListener() {
            @Override
            public void onAdLoaded() {
                Log.d(TAG, "onAdLoaded");
            }

            @Override
            public void onAdFailedToLoad(int errorCode) {
//                Toast.makeText(self, "banner id: " + errorCode, Toast.LENGTH_LONG).show();
                // Code to be executed when an ad request fails.
                Log.d(TAG, "onAdFailedToLoad: " + errorCode);
            }

            @Override
            public void onAdOpened() {
                // Code to be executed when an ad opens an overlay that
                // covers the screen.
            }

            @Override
            public void onAdClicked() {
                // Code to be executed when the user clicks on an ad.
            }

            @Override
            public void onAdLeftApplication() {
                // Code to be executed when the user has left the app.
            }

            @Override
            public void onAdClosed() {
                // Code to be executed when the user is about to return
                // to the app after tapping on an ad.
            }
        });
        AdRequest adRequest = new AdRequest.Builder().build();
        mAdView.loadAd(adRequest);
    }

    public void initRewardedAd() {
        rewardedAd_box_0 = createGoogleRewardedAd(test_id);
        rewardedAd_box_1 = createGoogleRewardedAd(test_id);
        rewardedAd_home_0 = createGoogleRewardedAd(test_id);
        rewardedAd_home_1 = createGoogleRewardedAd(test_id);
        rewardedAd_topUp_0 = createGoogleRewardedAd(test_id);
        rewardedAd_topUp_1 = createGoogleRewardedAd(test_id);
    }

    public static RewardedAd createGoogleRewardedAd(String adUnitId) {
        RewardedAd rewardedAd = new RewardedAd(self,
                adUnitId);
        RewardedAdLoadCallback adLoadCallback = new RewardedAdLoadCallback() {
            @Override
            public void onRewardedAdLoaded() {
//                Toast.makeText(self, "onRewardedAdLoaded", Toast.LENGTH_LONG).show();
            }

            @Override
            public void onRewardedAdFailedToLoad(int errorCode) {
//                Toast.makeText(self, "rewarded load id: " + errorCode, Toast.LENGTH_LONG).show();
//                Log.e("GoogleAd", "rewarded load id: " + errorCode);
            }
        };
        rewardedAd.loadAd(new AdRequest.Builder().build(), adLoadCallback);
        return rewardedAd;
    }

    public static void isValidRewarded(final String box_tag) {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                RewardedAd rewardedAd_box;
                if (box_tag == "box_0") {
                    rewardedAd_box = rewardedAd_box_0;
                } else if (box_tag == "box_1") {
                    rewardedAd_box = rewardedAd_box_1;
                } else if (box_tag == "home_0") {
                    rewardedAd_box = rewardedAd_home_0;
                } else if (box_tag == "home_1") {
                    rewardedAd_box = rewardedAd_home_1;
                } else if (box_tag == "topUp_0") {
                    rewardedAd_box = rewardedAd_topUp_0;
                } else if (box_tag == "topUp_1") {
                    rewardedAd_box = rewardedAd_topUp_1;
                } else {
                    return;
                }
                RewardedAdCallback adCallback = new RewardedAdCallback() {
                    @Override
                    public void onRewardedAdOpened() {
                    }
                    @Override
                    public void onRewardedAdClosed() {
                    }
                    @Override
                    public void onUserEarnedReward(@NonNull RewardItem rewardItem) {
                        self.runOnGLThread(new Runnable() {
                            @Override
                            public void run() {
                                Cocos2dxJavascriptJavaBridge.evalString("Global.getRewardedByAd()");
                            }
                        });
                    }
                    @Override
                    public void onRewardedAdFailedToShow(int errorCode) {
//                        Toast.makeText(self, "rewarded show id: " + errorCode, Toast.LENGTH_LONG).show();
                    }
                };
                if (rewardedAd_box.isLoaded()) {
                    rewardedAd_box.show(self, adCallback);
                } else {
                    if (box_tag == "box_0") {
                        FacebookAd.isValidRewarded("box_1");
                    } else if (box_tag == "box_1") {

                    } else if (box_tag == "home_0") {
                        FacebookAd.isValidRewarded("home_1");
                    } else if (box_tag == "home_1") {

                    } else if (box_tag == "topUp_0") {
                        FacebookAd.isValidRewarded("topUp_1");
                    } else if (box_tag == "topUp_1") {

                    }
                }
            }
        };
        self.runOnUiThread(runnable);
    }

// ---------------------------------------- 分割线 -------------------------------------------------
    public void initInterstitialAd() {
        interstitialAd_startGame_0 = createGoogleInterstitialAd(test_id_new);
        interstitialAd_startGame_1 = createGoogleInterstitialAd(test_id_new);
        interstitialAd_victory_0 = createGoogleInterstitialAd(test_id_new);
        interstitialAd_victory_1 = createGoogleInterstitialAd(test_id_new);
    }

    public static InterstitialAd createGoogleInterstitialAd(String adUnitId) {
        InterstitialAd mInterstitialAd = new InterstitialAd(self);
        mInterstitialAd.setAdUnitId(adUnitId);
        mInterstitialAd.setAdListener(new AdListener() {
            @Override
            public void onAdLoaded() {
            }

            @Override
            public void onAdFailedToLoad(int errorCode) {
                // Code to be executed when an ad request fails.
//                Toast.makeText(self, "interstitial id: " + errorCode, Toast.LENGTH_LONG).show();
            }

            @Override
            public void onAdOpened() {
                // Code to be executed when the ad is displayed.
            }

            @Override
            public void onAdClicked() {
                // Code to be executed when the user clicks on an ad.
            }

            @Override
            public void onAdLeftApplication() {
                // Code to be executed when the user has left the app.
            }

            @Override
            public void onAdClosed() {
                // Code to be executed when the interstitial ad is closed.
            }
        });

        mInterstitialAd.loadAd(new AdRequest.Builder().build());
        return mInterstitialAd;
    }

    public static void isValidInterstitial(final String box_tag) {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                InterstitialAd mInterstitialAd;
                if (box_tag == "victory_0") {
                    mInterstitialAd = interstitialAd_victory_0;
                } else if (box_tag == "victory_1") {
                    mInterstitialAd = interstitialAd_victory_1;
                } else if (box_tag == "startGame_0") {
                    mInterstitialAd = interstitialAd_startGame_0;
                } else if (box_tag == "startGame_1") {
                    mInterstitialAd = interstitialAd_startGame_1;
                } else {
                    return;
                }
                if (mInterstitialAd != null && mInterstitialAd.isLoaded()) {
                    mInterstitialAd.show();
                } else {
                    if (box_tag == "victory_0") {
                        FacebookAd.isValidInterstitial("victory_1");
                    } else if (box_tag == "victory_1") {

                    } else if (box_tag == "startGame_0") {
                        FacebookAd.isValidInterstitial("startGame_1");
                    } else if (box_tag == "startGame_1") {

                    }
                }
            }
        };
        self.runOnUiThread(runnable);
    }

    // ------------------------------------- 分割线 ------------------------------------------------
    public static void interstitialAdMgr() {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                if (interstitialAd_victory_0 != null && !interstitialAd_victory_0.isLoaded()) {
                    interstitialAd_victory_0 = createGoogleInterstitialAd(test_id_new);
                }
                if (interstitialAd_victory_1 != null && !interstitialAd_victory_1.isLoaded()) {
                    interstitialAd_victory_1 = createGoogleInterstitialAd(test_id_new);
                }
                if (interstitialAd_startGame_0 != null && !interstitialAd_startGame_0.isLoaded()) {
                    interstitialAd_startGame_0 = createGoogleInterstitialAd(test_id_new);
                }
                if (interstitialAd_startGame_1 != null && !interstitialAd_startGame_1.isLoaded()) {
                    interstitialAd_startGame_1 = createGoogleInterstitialAd(test_id_new);
                }
            }
        };
        self.runOnUiThread(runnable);
    }

    public static void rewardedAdMgr() {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                if (rewardedAd_box_0 != null && !rewardedAd_box_0.isLoaded()) {
                    rewardedAd_box_0 = createGoogleRewardedAd(test_id);
                }
                if (rewardedAd_box_1 != null && !rewardedAd_box_1.isLoaded()) {
                    rewardedAd_box_1 = createGoogleRewardedAd(test_id);
                }
                if (rewardedAd_home_0 != null && !rewardedAd_home_0.isLoaded()) {
                    rewardedAd_home_0 = createGoogleRewardedAd(test_id);
                }
                if (rewardedAd_home_1 != null && !rewardedAd_home_1.isLoaded()) {
                    rewardedAd_home_1 = createGoogleRewardedAd(test_id);
                }
                if (rewardedAd_topUp_0 != null && !rewardedAd_topUp_0.isLoaded()) {
                    rewardedAd_topUp_0 = createGoogleRewardedAd(test_id);
                }
                if (rewardedAd_topUp_1 != null && !rewardedAd_topUp_1.isLoaded()) {
                    rewardedAd_topUp_1 = createGoogleRewardedAd(test_id);
                }
            }
        };
        self.runOnUiThread(runnable);
    }
}

//    private AdSize getAdSize() {
//        // Step 2 - Determine the screen width (less decorations) to use for the ad width.
//        Display display = getWindowManager().getDefaultDisplay();
//        DisplayMetrics outMetrics = new DisplayMetrics();
//        display.getMetrics(outMetrics);
//
//        float widthPixels = outMetrics.widthPixels;
//        float density = outMetrics.density;
//
//        int adWidth = (int) (widthPixels / density);
//
//        // Step 3 - Get adaptive ad size and return for setting on the ad view.
//        return AdSize.getCurrentOrientationAnchoredAdaptiveBannerAdSize(this, adWidth);
//    }

```

