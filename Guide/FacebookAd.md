[FacebookAd教程网站](https://developers.facebook.com/docs/audience-network/get-started/android/?translation)

---

## 添加 SDK

* 应用层级（而不是项目层级！）的 build.gradle 添加下列编译语句

```java
dependencies {
  compile 'com.android.support:recyclerview-v7:25.3.1' // Required Dependency by Audience Network SDK
  compile 'com.facebook.android:audience-network-sdk:5.+'
}
```

## Sample

```java
package org.cocos2dx.javascript;

import android.app.Activity;
import android.util.Log;
import android.view.ViewGroup;
import android.widget.RelativeLayout;
import android.widget.Toast;

import com.facebook.ads.*;

import org.cocos2dx.lib.Cocos2dxJavascriptJavaBridge;

public class FacebookAd {
    private static final String[] rewarded_box_id = {"937472093426124_937473266759340", "937472093426124_938196486687018"};
    private static final String[] rewarded_home_id = {"937472093426124_938195506687116", "937472093426124_938848846621782"};
    private static final String[] rewarded_topUp_id = {"937472093426124_938195920020408", "937472093426124_938848716621795"};
    private static final String[] interstitial_victory_id = {"937472093426124_938851333288200", "937472093426124_937530403420293"};
    private static final String[] interstitial_startGame_id = {"937472093426124_938852226621444", "937472093426124_938851913288142"};
    private static final String banner_id = "937472093426124_937529343420399";
    private final String TAG = "FacebookAd";
    private static RewardedVideoAd rewardedVideoAd_box_0;
    private static RewardedVideoAd rewardedVideoAd_box_1;
    private static RewardedVideoAd rewardedVideoAd_home_0;
    private static RewardedVideoAd rewardedVideoAd_home_1;
    private static RewardedVideoAd rewardedVideoAd_topUp_0;
    private static RewardedVideoAd rewardedVideoAd_topUp_1;
    private static InterstitialAd interstitialAd_victory_0;
    private static InterstitialAd interstitialAd_victory_1;
    private static InterstitialAd interstitialAd_startGame_0;
    private static InterstitialAd interstitialAd_startGame_1;
    public AdView adView;
    private AppActivity self;
    protected RelativeLayout m_FrameLayout;
    private GoogleAd mGoogleAd;

    public FacebookAd(AppActivity appActivity, RelativeLayout mFrameLayout, GoogleAd googleAd) {
        AdSettings.addTestDevice("654a4448-03cf-4265-ab16-b0bae2e0de55");
        self = appActivity;
        m_FrameLayout = mFrameLayout;
        mGoogleAd = googleAd;
        AudienceNetworkAds.initialize(self);

        if (!self.isBuy) {
            facebookBanner();
        }

        initRewarded();
        initInterstitial();
    }

    public void facebookBanner() {
        adView = new AdView(self, banner_id, AdSize.BANNER_HEIGHT_50);
        adView.setAdListener(new AdListener() {
            @Override
            public void onError(Ad ad, AdError adError) {
                Log.d(TAG, "facebookBanner ErrorCode: " + adError.getErrorCode() + " info:" +  adError.getErrorMessage());
                mGoogleAd.googleBannerAd();
            }

            @Override
            public void onAdLoaded(Ad ad) {
                // Ad loaded callback
                Log.d(TAG, "banner onAdLoaded");
            }

            @Override
            public void onAdClicked(Ad ad) {
                Log.d(TAG, "banner onAdClicked");
            }

            @Override
            public void onLoggingImpression(Ad ad) {
                Log.d(TAG, "banner onLoggingImpression");
            }
        });
        RelativeLayout.LayoutParams params = new RelativeLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);
        params.addRule(RelativeLayout.ALIGN_PARENT_BOTTOM);
        params.addRule(RelativeLayout.CENTER_HORIZONTAL);
        m_FrameLayout.addView(adView,params);
        adView.loadAd();
    }

    public void initRewarded() {
        rewardedVideoAd_box_0 = createRewarded(rewarded_box_id[0]);
        rewardedVideoAd_box_1 = createRewarded(rewarded_box_id[1]);
        rewardedVideoAd_home_0 = createRewarded(rewarded_home_id[0]);
        rewardedVideoAd_home_1 = createRewarded(rewarded_home_id[1]);
        rewardedVideoAd_topUp_0 = createRewarded(rewarded_topUp_id[0]);
        rewardedVideoAd_topUp_1 = createRewarded(rewarded_topUp_id[1]);
    }

    public RewardedVideoAd createRewarded(String adId) {
        RewardedVideoAd rewardedVideoAd_box = new RewardedVideoAd(self, adId);
        rewardedVideoAd_box.setAdListener(new RewardedVideoAdListener() {
            @Override
            public void onError(Ad ad, AdError error) {
//                Log.e(TAG, "failed to load id: " + error.getErrorCode() + "info: "+ error.getErrorMessage());
            }

            @Override
            public void onAdLoaded(Ad ad) {
            }

            @Override
            public void onAdClicked(Ad ad) {
                Log.d(TAG, "Rewarded video ad clicked!");
            }

            @Override
            public void onLoggingImpression(Ad ad) {
//                Log.d(TAG, "Rewarded video ad impression logged!");
            }

            @Override
            public void onRewardedVideoCompleted() {
//                Log.d(TAG, "Rewarded video completed!");
                // giveReward();
                self.runOnGLThread(new Runnable() {
                    @Override
                    public void run() {
                        Cocos2dxJavascriptJavaBridge.evalString("Global.getRewardedByAd()");
                    }
                });
            }

            @Override
            public void onRewardedVideoClosed() {
//                Log.d(TAG, "Rewarded video ad closed!");
            }
        });
        rewardedVideoAd_box.loadAd();
        return rewardedVideoAd_box;
    }

    public static void isValidRewarded(String tag) {
        RewardedVideoAd rewardedVideoAd;
        if (tag == "box_0") {
            rewardedVideoAd = rewardedVideoAd_box_0;
        } else if (tag == "box_1") {
            rewardedVideoAd = rewardedVideoAd_box_1;
        } else if (tag == "home_0") {
            rewardedVideoAd = rewardedVideoAd_home_0;
        } else if (tag == "home_1") {
            rewardedVideoAd = rewardedVideoAd_home_1;
        } else if (tag == "topUp_0") {
            rewardedVideoAd = rewardedVideoAd_topUp_0;
        } else if (tag == "topUp_1") {
            rewardedVideoAd = rewardedVideoAd_topUp_1;
        } else {
            return;
        }
        if(rewardedVideoAd == null) {
        } else if (!rewardedVideoAd.isAdLoaded()) {
        } else if(rewardedVideoAd.isAdInvalidated()) {
        } else {
            rewardedVideoAd.show();
            return;
        }
        GoogleAd.isValidRewarded(tag);
    }

    // ------------------------------------- 分割线 ------------------------------------------------

    public void initInterstitial() {
        interstitialAd_victory_0 = createInterstitial(interstitial_victory_id[0]);
        interstitialAd_victory_1 = createInterstitial(interstitial_victory_id[1]);
        interstitialAd_startGame_0 = createInterstitial(interstitial_startGame_id[0]);
        interstitialAd_startGame_1 = createInterstitial(interstitial_startGame_id[1]);
    }

    public InterstitialAd createInterstitial(String adId) {
        InterstitialAd interstitialAd = new InterstitialAd(self, adId);
        interstitialAd.setAdListener(new InterstitialAdListener() {
            @Override
            public void onInterstitialDisplayed(Ad ad) {
                // Interstitial ad displayed callback
//                Log.e(TAG, "Interstitial ad displayed.");
            }

            @Override
            public void onInterstitialDismissed(Ad ad) {
                // Interstitial dismissed callback
//                Log.e(TAG, "Interstitial ad dismissed.");
            }

            @Override
            public void onError(Ad ad, AdError adError) {
//                Log.e(TAG, "Interstitial ad failed to load: " + adError.getErrorMessage());
            }

            @Override
            public void onAdLoaded(Ad ad) {
                // Interstitial ad is loaded and ready to be displayed
//                Log.d(TAG, "Interstitial ad is loaded and ready to be displayed!");
            }

            @Override
            public void onAdClicked(Ad ad) {
                // Ad clicked callback
//                Log.d(TAG, "Interstitial ad clicked!");
            }

            @Override
            public void onLoggingImpression(Ad ad) {
                // Ad impression logged callback
//                Log.d(TAG, "Interstitial ad impression logged!");
            }
        });
        interstitialAd.loadAd();
        return interstitialAd;
    }

    public static void isValidInterstitial(String tag) {
        InterstitialAd interstitialAd;
        if (tag == "victory_0"){
            interstitialAd = interstitialAd_victory_0;
        } else if (tag == "victory_1") {
            interstitialAd = interstitialAd_victory_1;
        } else if (tag == "startGame_0"){
            interstitialAd = interstitialAd_startGame_0;
        } else if (tag == "startGame_1") {
            interstitialAd = interstitialAd_startGame_1;
        } else{
            return;
        }
        if(interstitialAd == null) {
        } else if(!interstitialAd.isAdLoaded()) {
        } else if(interstitialAd.isAdInvalidated()) {
        } else {
            interstitialAd.show();
            return;
        }
        GoogleAd.isValidInterstitial(tag);
    }

    // ------------------------------------- 分割线 ------------------------------------------------
    public static void interstitialAdMgr() {
        checkInterstitialAd(interstitialAd_victory_0);
        checkInterstitialAd(interstitialAd_victory_1);
        checkInterstitialAd(interstitialAd_startGame_0);
        checkInterstitialAd(interstitialAd_startGame_1);
    }

    public static void rewardedAdMgr() {
        checkRewardedAd(rewardedVideoAd_box_0);
        checkRewardedAd(rewardedVideoAd_box_1);
        checkRewardedAd(rewardedVideoAd_home_0);
        checkRewardedAd(rewardedVideoAd_home_1);
        checkRewardedAd(rewardedVideoAd_topUp_0);
        checkRewardedAd(rewardedVideoAd_topUp_1);
    }

    public static void checkInterstitialAd(InterstitialAd interstitialAd) {
        if(interstitialAd == null) {
        } else if(!interstitialAd.isAdLoaded()) {
            interstitialAd.loadAd();
        } else if(interstitialAd.isAdInvalidated()) {
            interstitialAd.loadAd();
        }
    }

    public static void checkRewardedAd(RewardedVideoAd rewardedVideoAd) {
        if(rewardedVideoAd == null) {
        } else if(!rewardedVideoAd.isAdLoaded()) {
            rewardedVideoAd.loadAd();
        } else if(rewardedVideoAd.isAdInvalidated()) {
            rewardedVideoAd.loadAd();
        }
    }
}

```

```java
package org.cocos2dx.javascript;

import org.cocos2dx.lib.Cocos2dxActivity;
import org.cocos2dx.lib.Cocos2dxGLSurfaceView;

import android.app.Activity;
import android.os.Bundle;

import android.content.Intent;
import android.content.res.Configuration;

import com.tendcloud.tenddata.TalkingDataGA;
import com.tendcloud.tenddata.TDGAAccount;
import com.tendcloud.tenddata.TDGAMission;
import android.content.SharedPreferences;
import android.util.Log;


public class AppActivity extends Cocos2dxActivity {
    static TDGAAccount account;
    private FacebookAd mFacebookAd;
    private GoogleAd mGoogleAd;
    private GooglePlay googlePlay;
    public boolean isBuy;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        loadData();
        mGoogleAd = new GoogleAd(AppActivity.this, mFrameLayout);
        mFacebookAd = new FacebookAd(AppActivity.this, mFrameLayout, mGoogleAd);
        googlePlay = new GooglePlay(AppActivity.this);
    }
    
    @Override
    protected void onDestroy() {
        if (googlePlay.billingClient.isReady()) {
            googlePlay.billingClient.endConnection();
        }
        super.onDestroy();
        SDKWrapper.getInstance().onDestroy();

    }
    void loadData() {
        SharedPreferences sp = getPreferences(MODE_PRIVATE);
        int max=1000000000,min=10000000;
        long randomNum = System.currentTimeMillis();  
        randomNum = (randomNum%(max-min)+min);
        String ran = Long.toString(randomNum);
        String userId = sp.getString("userId", ran);
        saveData(userId);
        account = TDGAAccount.setAccount(userId);

        isBuy = sp.getBoolean("isBuy", false);
    }

    void saveData(String userId) {
        SharedPreferences.Editor spe = getPreferences(MODE_PRIVATE).edit();
        spe.putString("userId", userId);
        spe.apply();
    }

    public void saveBuy() {
        if (!isBuy) {
            if (mFacebookAd.adView != null) {
                mFacebookAd.adView.destroy();
                mFacebookAd.adView = null;
            }

            if (mGoogleAd.mAdView != null) {
                mGoogleAd.mAdView.destroy();
                mGoogleAd.mAdView = null;
            }

            isBuy = true;
            SharedPreferences.Editor spe = getPreferences(MODE_PRIVATE).edit();
            spe.putBoolean("isBuy", true);
            spe.apply();
        }
    }

    public static void TalkingLevel(int level) {
        account.setLevel(level);
    }

    public static void rewardedAd(String tag) {
        FacebookAd.rewardedAdMgr();
        GoogleAd.rewardedAdMgr();
        if (tag.equals("box")) {
            FacebookAd.isValidRewarded("box_0");
        } else if (tag.equals("home")) {
            FacebookAd.isValidRewarded("home_0");
        } else if (tag.equals("topUp")) {
            FacebookAd.isValidRewarded("topUp_0");
        }
    }

    public static void InterstitialAd(String tag) {
        FacebookAd.interstitialAdMgr();
        GoogleAd.interstitialAdMgr();
        if (tag.equals("victory")) {
            FacebookAd.isValidInterstitial("victory_0");
        } else if (tag.equals("startGame")) {
            FacebookAd.isValidInterstitial("startGame_0");
        }
    }
}

```

