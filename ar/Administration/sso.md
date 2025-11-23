---
label: تسجيل الدخول الموحد (SSO)
icon: key
order: -20
route: /administration/sso/
---

# تسجيل الدخول الموحد (SSO)

يتيح لك SSO إنشاء مستخدمين وتأمين العديد من الصفحات المختلفة باستخدام بوابة تسجيل دخول معروضة على المواقع التي تريد تأمينها. على الرغم من أنه معقد في الإعداد، إلا أنه طريقة جيدة لتعلم SSO وتأمين نسخة ST الخاصة بك على الإنترنت بشكل أكبر.

يمكن لـ SSO أيضًا استبدال [المصادقة الأساسية لـ HTTP](/Administration/config-yaml.md#user-authentication) كآلية للتحكم في الوصول إلى [الاتصالات عن بعد](/Administration/remote-connections.md).

يوصى بذلك لأن SSO يوفر أمانًا ووظائف أفضل من المصادقة الأساسية لـ HTTP.

[**Authelia**](https://www.authelia.com/) و [**Authentik**](https://goauthentik.io/) هما موفرا SSO مفتوحا المصدر يمكن استخدامهما مع SillyTavern.

## تسجيل الدخول باستخدام SSO

إذا كان اسم المستخدم المقدم من SSO **يطابق تمامًا** معرف مستخدم حساب مستخدم SillyTavern، يمكنك تسجيل الدخول إلى SillyTavern كهذا المستخدم عبر SSO. لتمكين هذه الميزة، قم بتغيير أحد الخيارات التالية إلى ملف [config.yaml](/Administration/config-yaml.md#sso-auto-login) الخاص بك:

### Authelia

```yaml
sso:
  autheliaAuth: true
```

### Authentik

```yaml
sso:
  authentikAuth: true
```

كلا الخيارين يزيدان أو يستبدلان مكون [إدارة كلمات المرور](/Usage/User_Settings/index.md#account-management) المدمج في إعداد [الوضع متعدد المستخدمين](/Administration/multi-user.md).
