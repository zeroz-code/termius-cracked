# Termius-cracked

Unpacking `app.asar`
```shell
cd /Applications/Termius.app/Contents/Resources/
npm i -g @electron/asar #unpack environment configuration
asar extract app.asar ./app #no need to repack after modification
asar pack ./app app.asar #pack
mv app.asar app.asar.bak #keep a backup, or rm directly
rm app-update.yml #prevent automatic update
```

> update 2024.08.16

First install nodejs and configure `npm`<br/>

[Click here to download nodejs](https://nodejs.cn/download)

Install `asar`
```shell
npm install -g asar
```

## Decryption method 1
- Applicable to versions before 9.2.0

1. Modify `app\background-process`

Search for `await this.api.bulkAccount`

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

## Decryption method 2
- Applicable to 9.2.0 +

1. Unpacking `app.asar`
```shell
cd /Applications/Termius.app/Contents/Resources/
asar extract app.asar ./app # No need to repackage after modification
mv app.asar app.asar.bak # Keep a backup, or just rm
rm app-update.yml # Prevent automatic update
```

1. Remove login and remove restrictions<br>Revise `app\ui-process`

Find `Welcome Screen`
Modify as follows
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

Find `isprouser:` and modify it as follows

```js
 w7e = (t) => ({
    sort: t.sorting.snippets,
    snippetsPackages: L2(t),
    isprouser: L2(t),
    editableVaults: Dn(t),
  }),
```

2. Modify `app\ui-process`

Search for `function Gl(e)` Modify as follows

```js
function GL(e) {
return ture
}
// {
//   if (!e) return !1;
//   var t = e._valueTracker;
//   if (!t) return !0;
//   var n = t.getValue(),
//     r = "";
//   return (
//     e && (r = KL(e) ? (e.checked ? "true" : "false") : e.value),
//     (e = r),
//     e !== n ? (t.setValue(e), !0) : !1
//   );
// }
```

Search for `function Ol(e)` Modify as follows
```js
function Ol(e) {
return ture
}
// {
//   for (; e != null; e = e.nextSibling) {
//     var t = e.nodeType;
//     if (t === 1 || t === 3) break;
//     if (t === 8) {
//       if (((t = e.data), t === "$" || t === "$!" || t === "$?")) break;
//       if (t === "/$") return null;
//     }
//   }
//   return e;
// }
```
The best solution for step 2 above<br>
Removed account login and welcome screen every time it opens<br>
Search for `I already use Termius`
Modify as follows
```js
   switch (e) {
          case "firstIntroductionScreen":
            // n("log-in");
            // break;
          case "secondIntroductionScreen":
            l(Qo({ onPage: 1 })), n("firstIntroductionScreen");
            break;
          case "thirdIntroductionScreen":
            l(Qo({ onPage: 2 })), n("secondIntroductionScreen");
            break;
        }
      },
      d = () => {
        switch (e) {
          case "firstIntroductionScreen":
            l(Qo({ onPage: 2 })), n("secondIntroductionScreen");
            break;
          case "secondIntroductionScreen":
            l(Qo({ onPage: 3 })), n("thirdIntroductionScreen");
            break;
          case "thirdIntroductionScreen":
            // n("sign-up");
            // break;
          case "success":
            r();
            break;
        }
      },
```
