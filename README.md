## عنوان عقد PinkAntiBot:

1. MAINNET: 0xf4f071EB637b64fC78C9eA87DaCE4445D119CA35
2. BSC: 0x8EFDb3b642eb2a20607ffe0A56CFefF6a95Df002
3. BSC_TESTNET: 0xbb06F5C7689eA93d9DeACCf4aF8546C4Fe0Bf1E5
4. ماتيك: 0x56a79881b65B03F27b088B753B6c128485642FC3
5. KCC_MAINNET: 0x2A7F08C820f3382D38B855ba59ad26444938a2b5
6. AVAX: 0x18F349AD12d7d7f029B3b22e0B01c6D88a0D2066
7. FTM: 0xcA461AcF6A9E68FA6D53410eba43cefde7dF5466
8. CRONOS: 0x785A195F7b6a0dDaf7E41EBcBddE7a98F4Cb24A9

##  دليل التكامل PinkAntiBot

1.   أضف هذه الواجهة إلى قاعدة التعليمات البرمجية الخاصة بك: 

"IPinkAntiBot.sol"
"'صلابة.
// SPDX-License-Identifier: معهد ماساتشوستس للتكنولوجيا
براغما صلابة >=0.5.0;

واجهة IPinkAntiBot {
  دالة setTokenOwner (مالك العنوان) خارجي؛  

 وظيفة على PreTransferCheck ( 
 العنوان من،10,000,0000
 ) الخارجية؛ 
}
"'

2.  تحديث عقد الرمز المميز الخاص بك:

"الدافع.
+   استيراد "المسار / إلى / IPinkAntiBot.sol" ؛ 

عقد MyToken {
+ IPinkAntiBot العامة الورديAntiBot;

 المنشئ ( 
 اسم ذاكرة الخيط_, 
 رمز ذاكرة الوتر_، 
 UT8 الكسور العشرية_، 
 uint256 إجمالي العرض_، 
+ عنوان الورديAntiBot_ 
 ) { 
 ... حذفت من أجل الوضوح 

 // إنشاء مثال لمتغير PinkAntiBot من العنوان المقدم 
+ الوردي AntiBot = IPinkAntiBot (pinkAntiBot_) ؛ 
 // سجل الموزع ليكون مالك الرمز المميز مع PinkAntiBot. يمكنك. 
 // في وقت لاحق تغيير مالك الرمز المميز في عقد PinkAntiBot 
+ الورديAntiBot.setمالك الرمز المميز (msg.sender) ؛ 
 } 

 // داخل وظيفة _النقل في ERC20: 
 وظيفة _النقل ( 
 مرسل العنوان، 
 مستلم العنوان، 
 مبلغ UT256 
 ) الظاهري الداخلي { 
 تتطلب (المرسل!= العنوان (0) ، "ERC20: نقل من عنوان الصفر") ؛ 
 تتطلب (المتلقي!= العنوان (0) ، "ERC20: نقل إلى العنوان صفر") ؛ 
+ pinkAntiBot.onPreTransferCheck (المرسل، المتلقي، المبلغ) ؛ 
 } 
}
"'

3. زيارة https://www.pinksale.finance/#/antibot، تحديث الإعدادات الخاصة بك. بعد ذلك، يرجى تمكين Pink Anti-Bot (دفع رسوم 1 BNB في المرة الأولى)

![alt text](https://github.com/pinkmoonfinance/pink-antibot-guide/blob/main/pink-anti-bot-dashboard.png)

4. (Optional): If you want more control over how `PinkAntiBot` is enabled or disabled, you can do it inside your contract instead of relying on `PinkAntiBot` contract's configuration:


```diff
import "path/to/IPinkAntiBot.sol";

contract MyToken {
  IPinkAntiBot public pinkAntiBot;
+ bool public antiBotEnabled;

  constructor(
    string memory name_,
    string memory symbol_,
    uint8 decimals_,
    uint256 totalSupply_,
    address pinkAntiBot_ 
  ) {
    ... omitted for clarity

    // Create an instance of the PinkAntiBot variable from the provided address
    pinkAntiBot = IPinkAntiBot(pinkAntiBot_);
    // Register the deployer to be the token owner with PinkAntiBot. You can
    // later change the token owner in the PinkAntiBot contract
    pinkAntiBot.setTokenOwner(msg.sender);
+   antiBotEnabled = true;
  }

  // Use this function to control whether to use PinkAntiBot or not instead
  // of managing this in the PinkAntiBot contract
+ function setEnableAntiBot(bool _enable) external onlyOwner {
+   antiBotEnabled = _enable;
+ }

  // Inside ERC20's _transfer function:
  function _transfer(
    address sender,
    address recipient,
    uint256 amount
  ) internal virtual {
    require(sender != address(0), "ERC20: transfer from the zero address");
    require(recipient != address(0), "ERC20: transfer to the zero address");
    // Only use PinkAntiBot if this state is true
+   if (antiBotEnabled) {
      pinkAntiBot.onPreTransferCheck(sender, recipient, amount);
+   }
  }
}
```
