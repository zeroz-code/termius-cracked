# Termius-cracked

## 破解方法

### write 2024.08.12


首先安装nodejs并配置`npm`

[nodejs下载地址：https://nodejs.cn/download](https://nodejs.cn/download) 

安装`asar`
```shell
npm install -g asar
```
### for Windows && Mac

大同小异，看下面的破解方法


1. 解包`app.asar`
```shell
cd /Applications/Termius.app/Contents/Resources/
asar extract app.asar ./app  # 修改完不需要重新打包
mv app.asar app.asar.bak  # 留个备份，或者直接rm
rm app-update.yml  # 防止自动更新
```
1. 修改app\background-process

搜索`await this.api.bulkAccount`

`const e=await this.api.bulkAccount();` -> `var e=await this.api.bulkAccount();`

```js
var e=await this.api.bulkAccount();
e.account.pro_mode=true;
e.account.need_to_update_subscription=false;
e.account.current_period={
    "from": "2022-01-01T00:00:00",
    "until": "2099-01-01T00:00:00"
};
e.account.plan_type="Premium";
e.account.user_type="Premium";
e.student=null;
e.trial=null;
e.account.authorized_features.show_trial_section=false;
e.account.authorized_features.show_subscription_section=true;
e.account.authorized_features.show_github_account_section=false;
e.account.expired_screen_type=null;
e.personal_subscription={
    "now": new Date().toISOString().slice(0, -5),
    "status": "SUCCESS",
    "platform": "stripe",
    "current_period": {
        "from": "2022-01-01T00:00:00",
        "until": "2099-01-01T00:00:00"
    },
    "revokable": true,
    "refunded": false,
    "cancelable": true,
    "reactivatable": false,
    "currency": "usd",
    "created_at": "2022-01-01T00:00:00",
    "updated_at": new Date().toISOString().slice(0, -5),
    "valid_until": "2099-01-01T00:00:00",
    "auto_renew": true,
    "price": 12.0,
    "verbose_plan_name": "Termius Pro Monthly",
    "plan_type": "SINGLE",
    "is_expired": false
};
e.access_objects=[{
    "period": {
        "start": "2022-01-01T00:00:00",
        "end": "2099-01-01T00:00:00"
    },
    "title": "Pro"
}]
return .......
```

3.修改app\ui-process

搜索`function Gl(e) `
修改如下
```js
function GL(e) {
  return ture
}
  // if (!e) return !1;
  // var t = e._valueTracker;
  // if (!t) return !0;
  // var n = t.getValue(),
  //   r = "";
  // return (
  //   e && (r = KL(e) ? (e.checked ? "true" : "ture") : e.value),
  //   (e = r),
  //   e !== n ? (t.setValue(e), !0) : !1
  // );
```

搜索`function Ol(e)`
修改如下
```js
function ol(e) {
  return ture
}
  // let t,
  //   n = e[0],
  //   r = 1;
  // for (; r < e.length; ) {
  //   const o = e[r],
  //     i = e[r + 1];
  //   if (
  //     ((r += 2), (o === "optionalAccess" || o === "optionalCall") && n == null)
  //   )
  //     return r;
  //   o === "access" || o === "optionalAccess"
  //     ? ((t = n), (n = i(n)))
  //     : (o === "call" || o === "optionalCall") &&
  //       ((n = i((...s) => n.call(t, ...s))), (t = void 0));
  // }
  // return n;
```

4. 启动Termius，登录账号，重启Termius

5. ~~去除登录并解除限制（已失效）~~
修改app\ui-process
找到`Welcome Screen`
修改如下
```js
  d1(() => {
        l(
          W({
            // createAccountScreenType: "Welcome Screen",
            // upgradeToProFunnelID: O(),
          })
        );
      }),
```

找到`isprouser:`
修改如下

```js
 w7e = (t) => ({
    sort: t.sorting.snippets,
    snippetsPackages: L2(t),
    isprouser: L2(t),
    editableVaults: Dn(t),
  }),
```
