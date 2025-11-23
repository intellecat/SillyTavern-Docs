---
order: 10
route: /usage/api-connections/openrouter/
---

# OpenRouter

!!!info
OpenRouter متاح كمصدر Text Completion و Chat Completion. جميع النماذج متاحة من خلال أي من واجهتي API، لكن ميزاتها قد تختلف اعتماداً على نوع API الذي تختاره. على سبيل المثال، إدراج الصور واستدعاء الأدوات متاحة فقط مع Chat Completion API.
!!!

لا تريد الاشتراك في عشرات خدمات API، ولكن لا تزال تريد الوصول إلى جميع النماذج الأحدث؟ استخدم OpenRouter.

يعمل OpenRouter عن طريق السماح لك باستخدام نقطة نهاية واحدة للوصول إلى نماذج مثل DeepSeek و Claude و Gemini، كلها في خدمة واحدة مع تجمع رصيد مشترك.

لديها نسخة تجريبية مجانية (حوالي 1 دولار) ووصول مدفوع بعد ذلك. لا يوجد اشتراك أو فاتورة شهرية - تدفع مقابل ما تستخدمه فعلياً. بعض النماذج لديها وصول مجاني مع عدد محدود من الطلبات اليومية.

!!!tip
للحصول على وصول دائم إلى نماذج مجانية مع حد يومي سخي، تحتاج إلى شراء ما لا يقل عن 10 دولارات من الرصيد **مرة واحدة**.

راجع المزيد من التفاصيل على [صفحة OpenRouter FAQ](https://openrouter.ai/docs/faq).
!!!

- أنشئ حساب OpenRouter: [openrouter.ai](https://openrouter.ai/)
- [قائمة نماذج OpenRouter](https://openrouter.ai/models?order=pricing-low-to-high)

![OpenRouter-ConnectionPanel](/static/openrouter-connection.png)

من الأعلى إلى الأسفل (راجع الصورة أعلاه):

1. حدد API 'Chat Completion'.
2. حدد OpenRouter كمصدر.
3. انقر على "Authorize" للحصول على مفتاح باستخدام تدفق OAuth. بدلاً من ذلك، قم بإنشاء مفتاح API [هنا](https://openrouter.ai/keys) والصقه في المربع.
4. انقر على "Connect" وحدد نموذجاً.
5. (اختياري) استخدم زر "Test Message" للتحقق من اتصالك.
