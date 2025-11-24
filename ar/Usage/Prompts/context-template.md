---
order: 90
templating: false
route: /usage/prompts/context-template/
---

# Context Template

!!! ينطبق على: Text Completion APIs
للإعدادات المكافئة في Chat Completion APIs، استخدم [Prompt Manager](prompt-manager.md).
!!!

عادةً، تتطلب نماذج الذكاء الاصطناعي منك توفير بيانات الشخصية لها بطريقة محددة. يتضمن SillyTavern قائمة من قواعد التحويل المُعدة مسبقاً لنماذج مختلفة، ولكن يمكنك تخصيصها بالطريقة التي تريدها.

قم بتحرير هذه الإعدادات في لوحة "[Advanced Formatting](advancedformatting.md)".

## Story String

هذا الحقل هو template لمقدمة prompt (المعروفة داخلياً باسم story string). هذه هي الطريقة الرئيسية لإضافة المعلومات المحددة في [Character Cards](/Usage/Characters/index.md) لـ text completion و instruct models.

يدعم template بناء جملة Handlebars، وحقن نصوص مخصصة أو تنسيق، وأي [macros](/Usage/Characters/macros.md) أخرى. راجع مرجع اللغة هنا: <https://handlebarsjs.com/guide/>

نوفر المعاملات التالية لمُقيّم Handlebars (ملفوفة في أقواس معقوفة مزدوجة):

1. `{{anchorBefore}}`: Prompts المضبوطة لاستخدام موضع "Before Story String".
2. `{{anchorAfter}}`: Prompts المضبوطة لاستخدام موضع "After Story String".
3. `{{description}}`: [Description](/Usage/Characters/characterdesign.md#character-description) الخاص بالشخصية.
4. `{{scenario}}`: [Scenario](/Usage/Characters/characterdesign.md#scenario) الخاص بالشخصية.
5. `{{personality}}`: [Personality](/Usage/Characters/characterdesign.md#personality-summary) الخاص بالشخصية.
6. `{{system}}`: [system prompt](advancedformatting.md#system-prompt) أو تجاوز [main prompt](/Usage/Characters/characterdesign.md#prompt-overrides) الخاص بالشخصية (إذا كان موجوداً وتم تمكين "Prefer Char. Prompt" في User Settings).
7. `{{persona}}`: [وصف persona](/Usage/personas.md#persona-description) المحدد.
8. `{{char}}`: اسم الشخصية.
9. `{{user}}`: اسم persona المحدد.
10. `{{wiBefore}}` أو `{{loreBefore}}`: مدخلات [World Info](/Usage/worldinfo.md) المنشطة المجمعة مع Position مضبوط على "Before Char Defs".
11. `{{wiAfter}}` أو `{{loreAfter}}`: مدخلات [World Info](/Usage/worldinfo.md) المنشطة المجمعة مع Position مضبوط على "After Char Defs".
12. `{{mesExamples}}`: (اختياري) [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue) الخاصة بالشخصية، منسقة بـ instruct مع فاصل.
13. `{{mesExamplesRaw}}`: [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue) الخاصة بالشخصية بتنسيق خام، بدون أي تنسيق.

!!!tip **مهم**
عند استخدام `{{mesExamples}}` في Story String، اضبط **"Example Messages Behavior"** في لوحة **<i class="fa-solid fa-user-cog"></i> User Settings** على **"Never include examples"** لتجنب تكرار رسائل الأمثلة في prompt.
!!!

يتم دعم macro خاص `{{trim}}` لإزالة أي أسطر جديدة تحيط به. استخدمه إذا كنت تريد أن لا يكون جزء من النص مفصولاً عن السطر السابق بسطر جديد (_لا يتم قص المسافات_).

**تحذير**: إذا كان أي من المعاملات أعلاه مفقوداً من story string template، فلن يتم إرساله في prompt على الإطلاق.

### Prompt Anchors

الـ `{{anchorBefore}}` و `{{anchorAfter}}` هي عناصر نائبة عامة لـ prompts المضافة بواسطة إضافات مختلفة وميزات متنوعة في موضع ثابت مختار، على سبيل المثال:

* [Author's Note](/Usage/Characters/Author's-Note.md)
* [Summaries](/extensions/Summarize.md)
* [Chat Vectorization](/extensions/Chat-vectorization.md) / [Data Bank](/Usage/Characters/data-bank.md)
* [STscript injections](/For_Contributors/st-script.md#prompt-injections)
* [Web Search](/extensions/WebSearch.md)

### Story String position

بشكل افتراضي، يتم وضع story string المُقدم (مع استبدال جميع العناصر النائبة) في بداية prompt، متبوعاً برسائل الأمثلة وسجل المحادثة المرئي.

بدلاً من ذلك، يمكنك نقله إلى موضع ديناميكي عن طريق اختيار خيار "In-chat @ Depth"، والذي يضع story string على عمق محدد في سياق المحادثة.

!!!warning **انتباه**
إذا كان template يحتوي على عناصر prompt ثابتة (بادئات أو لواحق خاصة بالنموذج) لتغليف story string، فإن استخدام موضع "In-Chat @ Depth" سيؤدي إلى تغليفه بشكل غير صحيح مرتين مع تسلسلات مكررة، مما قد يؤدي إلى نتائج غير متوقعة.

في هذه الحالة، يمكنك إصلاح المشكلة بإحدى الطرق التالية:

1. **Templates مدمجة**: أعد تعيين templates إلى إعداداتها الافتراضية باستخدام الخطوات الموضحة في [Advanced Formatting](/Usage/Prompts/advancedformatting.md#resetting-templates).
2. **Templates مخصصة**: انقل العناصر الثابتة من story string template إلى [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
!!!

### Story String wrapping

!!!
القسم التالي ينطبق فقط عندما يكون **Instruct Mode** قيد التشغيل.
!!!

* موضع **Default**: سيتم تغليف Story String المُقدم باستخدام التسلسلات المحددة في [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
* موضع **In-chat @ Depth**: سيتم تغليف Story String المُقدم باستخدام التسلسلات المحددة في [Chat Messages Sequences](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping) لدور مختار (افتراضي: System).

## Example Separator

يُستخدم كرأس كتلة وفاصل بين كتل حوار الأمثلة. سيتم استبدال أي مثيل لعلامات `<START>` في حوارات الأمثلة بمحتويات هذا الحقل.

## Chat Start

يُدرج كفاصل بعد story string المُقدم وبعد كتل حوارات الأمثلة، ولكن قبل الرسالة الأولى في السياق.

## Separators as Stop Strings

يضيف "Example Separator" و "Chat Start" إلى قائمة stop strings.

مفيد إذا كان النموذج يميل إلى الهلوسة أو تسريب كتل كاملة من حوار الأمثلة مسبوقة بالفاصل.

## Names as Stop Strings

يضيف أسماء Character و User Persona إلى قائمة stop strings.

يُنصح بإبقائه مشغلاً لمنع انتحال النموذج.

## Always add character's name to prompt

!!!info
هذا الإعداد ليس له تأثير عندما يكون Instruct Mode قيد التشغيل. يتم تحديد سلوك الاسم بدلاً من ذلك بواسطة خيار [Include Names](/Usage/Prompts/instructmode.md#include-names) المحدد.
!!!

يلحق اسم الشخصية بـ prompt لإجبار النموذج على إكمال الرسالة كالشخصية:

```txt
** OTHER CONTEXT HERE **
Character:
```
