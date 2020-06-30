[GooglePlay教程网站](https://developer.android.google.cn/google/play/billing/billing_library_overview)

## Sample

* AppActivity.java 参考 FacebookAd

```java
package org.cocos2dx.javascript;

import android.util.Log;

import androidx.lifecycle.MutableLiveData;

import com.android.billingclient.api.BillingClient;
import com.android.billingclient.api.BillingClientStateListener;
import com.android.billingclient.api.BillingFlowParams;
import com.android.billingclient.api.BillingResult;
import com.android.billingclient.api.ConsumeParams;
import com.android.billingclient.api.ConsumeResponseListener;
import com.android.billingclient.api.Purchase;
import com.android.billingclient.api.PurchasesUpdatedListener;
import com.android.billingclient.api.SkuDetails;
import com.android.billingclient.api.SkuDetailsParams;
import com.android.billingclient.api.SkuDetailsResponseListener;

import org.cocos2dx.lib.Cocos2dxJavascriptJavaBridge;

import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class GooglePlay implements PurchasesUpdatedListener,BillingClientStateListener,SkuDetailsResponseListener {
    private static AppActivity self;
    public static BillingClient billingClient;
    private static final String TAG = "GooglePlay";
    public MutableLiveData<Map<String, SkuDetails>> skusWithSkuDetails = new MutableLiveData<>();
    public static String prices = "";
    public static Map<String, SkuDetails> newSkusDetailList = new HashMap<String, SkuDetails>();
// ------------------------------------ 初始化 -----------------------------------------------------
    public GooglePlay(AppActivity activity) {
        self = activity;
        billingClient = BillingClient.newBuilder(self)
                .setListener(this)
                .enablePendingPurchases() // Not used for subscriptions.
                .build();
        if (!billingClient.isReady()) {
            Log.d(TAG, "BillingClient: Start connection...");
            billingClient.startConnection(this);
        }
    }

    // ----------------------------------- 连接到 Google Play --------------------------------------
    @Override
    public void onBillingSetupFinished(BillingResult billingResult) {
        int responseCode = billingResult.getResponseCode();
        String debugMessage = billingResult.getDebugMessage();
        Log.d(TAG, "onBillingSetupFinished: " + responseCode + " " + debugMessage);
        if (responseCode == BillingClient.BillingResponseCode.OK) {
            // The billing client is ready. You can query purchases here.
            querySkuDetails();
//            queryPurchases();
        }
    }

    @Override
    public void onBillingServiceDisconnected() {
        Log.d(TAG, "onBillingServiceDisconnected");
        // TODO: Try connecting again with exponential backoff.
    }

// ---------------------------------------- 查询商品------------------------------------------------
    public void querySkuDetails() {
        // 查询商品
        Log.d(TAG, "querySkuDetails");
        List<String> skus = new ArrayList<>();
        skus.add("1");
        skus.add("2");
        skus.add("3");
        skus.add("4");
        skus.add("5");
        skus.add("6");
        skus.add("7");
        SkuDetailsParams params = SkuDetailsParams.newBuilder()
                .setType(BillingClient.SkuType.INAPP)
                .setSkusList(skus)
                .build();

        Log.i(TAG, "querySkuDetailsAsync");
        billingClient.querySkuDetailsAsync(params, this);
    }

    @Override
    public void onSkuDetailsResponse(BillingResult billingResult, List<SkuDetails> skuDetailsList) {
        // 查询商品的回调函数
        if (billingResult == null) {
            Log.wtf(TAG, "onSkuDetailsResponse: null BillingResult");
            return;
        }

        int responseCode = billingResult.getResponseCode();
        String debugMessage = billingResult.getDebugMessage();
        switch (responseCode) {
            case BillingClient.BillingResponseCode.OK:
                Log.i(TAG, "onSkuDetailsResponse4: " + responseCode + " " + debugMessage);
                if (skuDetailsList == null) {
                    Log.w(TAG, "onSkuDetailsResponse: null SkuDetails list");
                    skusWithSkuDetails.postValue(Collections.<String, SkuDetails>emptyMap());
                } else {
                    for (SkuDetails skuDetails : skuDetailsList) {
                        newSkusDetailList.put(skuDetails.getSku(), skuDetails);
                        String sku = skuDetails.getSku();
                        String price = skuDetails.getPrice();
                        Log.i(TAG, sku + " -- " + price);
                        prices = prices + "|" + price;
                    }
                    Log.i(TAG, prices);
                    skusWithSkuDetails.postValue(newSkusDetailList);
                    Log.i(TAG, "onSkuDetailsResponse: count " + newSkusDetailList.size());
                }
                break;
            case BillingClient.BillingResponseCode.SERVICE_DISCONNECTED:
            case BillingClient.BillingResponseCode.SERVICE_UNAVAILABLE:
            case BillingClient.BillingResponseCode.BILLING_UNAVAILABLE:
            case BillingClient.BillingResponseCode.ITEM_UNAVAILABLE:
            case BillingClient.BillingResponseCode.DEVELOPER_ERROR:
            case BillingClient.BillingResponseCode.ERROR:
                Log.e(TAG, "onSkuDetailsResponse3: " + responseCode + " " + debugMessage);
                break;
            case BillingClient.BillingResponseCode.USER_CANCELED:
                Log.i(TAG, "onSkuDetailsResponse2: " + responseCode + " " + debugMessage);
                break;
            // These response codes are not expected.
            case BillingClient.BillingResponseCode.FEATURE_NOT_SUPPORTED:
            case BillingClient.BillingResponseCode.ITEM_ALREADY_OWNED:
            case BillingClient.BillingResponseCode.ITEM_NOT_OWNED:
            default:
                Log.wtf(TAG, "onSkuDetailsResponse1: " + responseCode + " " + debugMessage);
        }
    }

// ------------------------------------- 购买商品 --------------------------------------------------
    public void handlePurchase(final Purchase purchase) {
        Log.d(TAG, "handlePurchase: "  + purchase.getSku());
        if (purchase.getPurchaseState() == Purchase.PurchaseState.PURCHASED) {
            ConsumeParams consumeParams =
                    ConsumeParams.newBuilder()
                            .setPurchaseToken(purchase.getPurchaseToken())
                            .setDeveloperPayload(purchase.getDeveloperPayload())
                            .build();
            billingClient.consumeAsync(consumeParams, new ConsumeResponseListener() {
                @Override
                public void onConsumeResponse(BillingResult billingResult, String purchaseToken) {
                    if (billingResult.getResponseCode() == BillingClient.BillingResponseCode.OK) {
                        Log.d(TAG, "消耗购买商品 "  + purchaseToken);
                    }
                }
            });
        }

        self.saveBuy();
        final String goods_sku = purchase.getSku();
        self.runOnGLThread(new Runnable() {
            @Override
            public void run() {
                Cocos2dxJavascriptJavaBridge.evalString("Global.getBuyGold(" + goods_sku + ")");
            }
        });
    }

    public void onPurchasesUpdated(BillingResult billingResult, List<Purchase> purchases) {
        if (billingResult == null) {
            Log.wtf(TAG, "onPurchasesUpdated: null BillingResult");
            return;
        }
        int responseCode = billingResult.getResponseCode();
        String debugMessage = billingResult.getDebugMessage();
        Log.d(TAG, "onPurchasesUpdated: " + responseCode + " " + debugMessage);
        switch (responseCode) {
            case BillingClient.BillingResponseCode.OK:
                if (purchases == null) {
                    Log.d(TAG, "onPurchasesUpdated: null purchase list");
//                    processPurchases(null);
                } else {
                    Log.d(TAG, "onPurchasesUpdated: purchases");
//                    processPurchases(purchases);
                    Log.d(TAG, "大小" + purchases.size());
                    for (Purchase purchase : purchases) {
                        handlePurchase(purchase);
                    }
                }
                break;
            case BillingClient.BillingResponseCode.USER_CANCELED:
                Log.i(TAG, "onPurchasesUpdated: User canceled the purchase");
                break;
            case BillingClient.BillingResponseCode.ITEM_ALREADY_OWNED:
                Log.i(TAG, "onPurchasesUpdated: The user already owns this item");
                break;
            case BillingClient.BillingResponseCode.DEVELOPER_ERROR:
                Log.e(TAG, "onPurchasesUpdated: Developer error means that Google Play " +
                        "does not recognize the configuration. If you are just getting started, " +
                        "make sure you have configured the application correctly in the " +
                        "Google Play Console. The SKU product ID must match and the APK you " +
                        "are using must be signed with release keys."
                );
                break;
        }
    }

    public static int launchBillingFlow(BillingFlowParams params) {
        if (!billingClient.isReady()) {
            Log.e(TAG, "launchBillingFlow: BillingClient is not ready");
            self.runOnGLThread(new Runnable() {
                @Override
                public void run() {
                    Cocos2dxJavascriptJavaBridge.evalString("Global.displayShopSever()");
                }
            });
            // Service connection is disconnected
        }
        BillingResult billingResult = billingClient.launchBillingFlow(self, params);
        int responseCode = billingResult.getResponseCode();
        String debugMessage = billingResult.getDebugMessage();
        Log.d(TAG, "launchBillingFlow: BillingResponse " + responseCode + " " + debugMessage);
        return responseCode;
    }

// --------------------------------------- JavaScript ----------------------------------------------
    public static String getPrices() {
        return prices;
    }

    public static void buyGold(String tag) {
        BillingFlowParams flowParams = BillingFlowParams.newBuilder()
                .setSkuDetails(newSkusDetailList.get(tag))
                .build();
        launchBillingFlow(flowParams);

    }
}

```

