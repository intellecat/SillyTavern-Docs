---
order: 100
icon: person-fill
route: /usage/characters/
---

# الشخصيات

الشخصيات هي هويات الذكاء الاصطناعي التي يمكنك إنشاؤها وإدارتها لتشكيل دور الذكاء الاصطناعي في المحادثة. كل شخصية لها اسم وشخصية وسجل محادثة. يمكنك إنشاء العدد الذي تريده من الشخصيات والتبديل بينها في أي وقت.

يمكن استخدام الشخصيات في المحادثات الفردية، أو إضافة شخصيات متعددة إلى محادثة جماعية لجعلها تتفاعل مع بعضها البعض.

## لوحة إدارة الشخصيات

افتح لوحة <i class="fa-solid fa-address-card"></i> **Characters** من شريط التنقل للوصول إلى قائمة الشخصيات. انقر على شخصية أو مجموعة للدردشة معها أو تعديلها، أو اختر <i class="fa-solid fa-user-plus"></i> **Create New Character** لإضافة شخصية جديدة.

### عناصر التحكم في اللوحة

* <i class="fa-solid fa-lock"></i> **Pin Panel**: إبقاء اللوحة مفتوحة أثناء التفاعل
* <i class="fa-solid fa-list-ul"></i> **Character List**: العودة إلى عرض قائمة الشخصيات
* **HotSwap Bar**: وصول سريع إلى الشخصيات المفضلة

### قائمة الشخصيات

* <i class="fa-solid fa-user-plus"></i> **Create New Character**: إضافة شخصية جديدة
* <i class="fa-solid fa-file-import"></i> **Import Character**: تحميل شخصية من ملف
* <i class="fa-solid fa-cloud-arrow-down"></i> **External Import**: استيراد من URL
* <i class="fa-solid fa-users-gear"></i> **Create Group**: بدء محادثة جماعية جديدة

#### البحث والفرز

* **Search Bar**: تصفية الشخصيات حسب الاسم أو الخصائص
* **Sort Dropdown**: خيارات فرز متعددة:
    - أبجديًا (A-Z, Z-A)
    - زمنيًا (الأحدث، الأقدم)
    - حسب الاستخدام (الأحدث، الأكثر/الأقل محادثات)
    - حسب الحجم (الأكثر/الأقل tokens)
    - خاص (المفضلة، عشوائي)

#### تصفية الشخصيات حسب النوع أو الوسم

* <i class="fa-solid fa-star"></i> **Favorites Filter**: عرض الشخصيات المفضلة
* <i class="fa-solid fa-users"></i> **Groups Filter**: عرض المحادثات الجماعية فقط
* <i class="fa-solid fa-folder-plus"></i> **Tags as Folders**: التنظيم حسب التسلسل الهرمي للوسوم
* <i class="fa-solid fa-gear"></i> **Manage Tags**: [إعداد الوسوم](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **Tag List**: عرض جميع الوسوم المتاحة
* <i class="fa-solid fa-filter-circle-xmark"></i> **Clear Filters**: إعادة تعيين جميع المرشحات

### لوحة إنشاء/تعديل الشخصية

* **Avatar Image**: رفع ومعاينة صورة الملف الشخصي للشخصية
* **Token Count**: [استخدام Token](characterdesign.md#character-tokens) للشخصية
* <i class="fa-solid fa-ranking-star"></i> **Stats**: سجل المحادثة وإحصائيات الاستخدام
* [إدارة الوسوم](/Usage/Characters/Tags.md)

#### الإجراءات السريعة

- <i class="fa-solid fa-star"></i> تبديل المفضلة
- <i class="fa-solid fa-book"></i> التعريفات المتقدمة
- <i class="fa-solid fa-globe"></i> قصة الشخصية
- <i class="fa-solid fa-passport"></i> قصة المحادثة: ربط المحادثة بـ [World Info](/Usage/worldinfo.md)
- <i class="fa-solid fa-file-export"></i> تصدير الشخصية
- <i class="fa-solid fa-clone"></i> نسخ
- <i class="fa-solid fa-skull"></i> حذف

#### الخيارات الموسعة

* ربط World Info
* استيراد قصة البطاقة
* تجاوز السيناريو
* تحويل الشخصية
* إعادة تسمية الشخصية
* ربط المصدر
* استبدال/تحديث
* استيراد الوسوم
* عرض المعرض

#### حقول المحتوى

* **[وصف الشخصية](characterdesign.md#character-description)**: ملخص موجز للشخصية
* **[الرسالة الأولى](characterdesign.md#first-message)**: تحية أولية أو موجه عند بدء محادثة جديدة
* **التحيات البديلة**: تحديد رسائل أولى متعددة يمكنك التمرير بينها عند بدء محادثة

### لوحة التعريفات المتقدمة

انقر على زر <i class="fa-solid fa-book"></i> **Advanced Definitions** للوصول إلى إعدادات الشخصية الموسعة.

#### تجاوزات الموجه (Chat Completion/Instruct Mode)

* **Main Prompt**: يستبدل [الموجه الرئيسي/النظام](/Usage/Prompts/index.md#main-prompt-system-prompt) الافتراضي، يمكن استخدام \{\{original\}\} placeholder لتضمين الموجه الأصلي
* **Post-History Instructions**: يتجاوز [تعليمات ما بعد السجل](/Usage/Prompts/index.md#post-history-instructions) الافتراضية

#### البيانات الوصفية للمنشئ

معلومات غير متعلقة بالموجه حول الشخصية:

- اسم/جهة اتصال المنشئ
- رقم إصدار الشخصية
- ملاحظات المنشئ
- قائمة الوسوم المضمنة

#### شخصية الشخصية

* **[ملخص الشخصية](characterdesign.md#personality-summary)**: نظرة عامة موجزة على صفات الشخصية
* **[السيناريو](characterdesign.md#scenario)**: سياق وظروف الحوار
* **ملاحظة الشخصية**: رسالة مخصصة مع عمق قابل للاختيار ودور الرسالة (انظر أيضًا [Author's Note](/Usage/Characters/Author's-Note.md))
* **Talkativeness** (المحادثات الجماعية): شريط تمرير من خجول ← عادي ← ثرثار
* **رسائل الأمثلة**: أمثلة على أسلوب كتابة الشخصية

### إدارة المحادثات الجماعية

إذا كانت هذه محادثة جماعية، يمكنك إدارة أعضاء المجموعة والإعدادات من هذه اللوحة.

راجع [المحادثات الجماعية](/Usage/Characters/groupchats.md) لمزيد من التفاصيل.
