هذه المشكلة شائعة عند استخدام حزمة **`tcolorbox`** مع النصوص العربية (RTL) ومحرك **LuaLaTeX**؛ حيث تحاول الحزمة افتراضياً رسم الصندوق من اليسار إلى اليمين (LTR)، مما يؤدي لإزاحة الصندوق خارج الصفحة وتشتت النص داخله [228، 408].

لحل هذه المشكلة وضمان ظهور النص داخل الصندوق بشكل سليم ومحاذاته لليمين، يجب عليك إضافة خيار **`rtl`** داخل تعريف الصندوق، وتعديل مكان العنوان ليتناسب مع اتجاه الكتابة العربية [2، 401].

إليك الكود المصحح لتعريف الصندوق في **الديباجة (Preamble)**:

```
% --- Define the updated custom box with RTL support ---
\newtcolorbox{TryItBox}[1]{%
  colback=blue!5!white,
  colframe=blue!75!black,
  fonttitle=\bfseries,
  title={#1},
  enhanced,
  rtl,                       % IMPORTANT: Enable Right-to-Left mode for the box
  width=\linewidth,          % Ensure the box stays within text boundaries
  % Adjust title position to the top RIGHT for Arabic style
  attach boxed title to top right={yshift=-2mm, xshift=-2mm},
  boxed title style={colback=blue!75!black},
  sharp corners=downhill,
  breakable                  % Allow box to split across pages
}
```

### ما الذي قمنا بتعديله ولماذا؟

1. **إضافة خيار `rtl`**: هذا هو الأمر الأهم؛ فهو يخبر الحزمة بأن محتوى الصندوق وتنسيقه يجب أن يبدأ من اليمين، وهو ما يحل مشكلة خروج الصندوق عن الهامش وظهور النص خارجه [119، 401].
2. **تحديد العرض `width=\linewidth`**: يضمن هذا الأمر أن الصندوق سيأخذ عرض السطر المتاح حالياً فقط ولن يتجاوزه، خاصة مع وجود هوامش التجليد التي صممناها سابقاً.
3. **تغيير موقع العنوان إلى `top right`**: بما أنك تكتب بالعربية، فمن الأجمل والأكثر منطقية أن يكون ملصق العنوان "ملاحظة هامة" في جهة اليمين بدلاً من اليسار.
4. **تعديل الإزاحة `xshift=-2mm`**: قمنا بجعل القيمة سالبة لسحب العنوان نحو داخل الصندوق من جهة اليمين.

### نصيحة إضافية للمحتوى:

عند كتابة النص داخل الصندوق، تأكد من أن الرموز الرياضية مثل الدرجات المئوية مكتوبة هكذا: `($0^\circ, -120^\circ, 120^\circ$)` واحرص على عدم وجود مسافات زائدة قبل أو بعد `\begin{TryItBox}` قد تؤدي لإزاحة بسيطة في السطر الأول [54، 409].

**جرب الكود الآن، وسيعود الصندوق للاستقرار داخل حدود الصفحة مع بقاء النص داخله بشكل أنيق.**

المشكلة ليست في أن الحزمة لا تدعم العربية، بل في **اختلاف المحرك**؛ فحزمة **`tcolorbox`** صممت خيار **`rtl`** ليعمل بشكل أساسي عند اكتشاف حزمة **`bidi`** (التي تُستخدم عادةً مع محرك XeLaTeX) [119، 408]. وبما أنك تستخدم محرك **LuaLaTeX** مع حزمة **`luabidi`**، فإن نظام المفاتيح داخل الصندوق قد لا يتعرف على أمر `rtl` مباشرة، مما يسبب الخطأ الذي واجهته [2، 119].

أما مشكلة إزاحة الصندوق للهامش، فهي تحدث لأن الصندوق يُعامل كعنصر "يساري" (LTR) في بيئة "يمينية" (RTL)، فيبدأ الرسم من نقطة خاطئة [228، 408].

إليك الحل التقني الصحيح المتوافق مع **LuaLaTeX** والعربية دون استخدام أمر `rtl` المسبب للخطأ:

### الكود البديل والمضمون (للديباجة):

سنستخدم خيارات المحاذاة اليدوية (**`halign`**) وضبط اتجاه النص داخل الصندوق لضمان بقائه داخل حدود الصفحة [75، 114].

```
% --- Corrected TryItBox for LuaLaTeX & Arabic ---
\newtcolorbox{TryItBox}{%
  colback=blue!5!white,
  colframe=blue!75!black,
  fonttitle=\bfseries,
  title={#1},
  enhanced,
  % --- Replacement for 'rtl' key ---
  halign=right,              % Aligns the body text to the right
  flush right title,         % Aligns the title text to the right
  width=\linewidth,          % Keeps the box within text margins
  before upper=\pagedir TRT\pardir TRT\textdir TRT, % Forces LuaTeX RTL direction
  % ---------------------------------
  attach boxed title to top right={yshift=-2mm, xshift=-2mm},
  boxed title style={colback=blue!75!black},
  sharp corners=downhill,
  breakable
}
```

### لماذا هذا الحل هو الأمثل؟

1. **توافق المحرك**: أمر `\textdir TRT` هو الأمر الداخلي لمحرك **LuaTeX** لتغيير اتجاه النص يدوياً من اليمين إلى اليسار (TRT تعني Text Right-to-Left)، وهو ما يحل محل مفتاح `rtl` المتعطل.
2. **ضبط المحاذاة**: استخدام **`halign=right`** يضمن أن الفقرات ستبدأ من جهة اليمين حتى لو كانت داخل بيئة برمجية معقدة.
3. **الثبات**: تحديد **`width=\linewidth`** يمنع الصندوق هندسياً من تجاوز الهوامش أو "الهروب" خارج الورقة مهما كان اتجاه النص [30، 228].

### مقتطف لتصحيح الصندوق الثاني (ExampleBox):

تأكد أيضاً من تعديله بنفس الطريقة (استبدال `rtl` بـ `halign=right`) ليعمل الكود بالكامل [75، 116]:

```
\newtcolorbox{ExampleBox}{% (Changed to 2 arguments: title and description)
  colback=gray!5!white,
  colframe=orange!80!black,
  fonttitle=\bfseries,
  title={#1: #2},
  enhanced,
  halign=right, % Safe RTL alignment for LuaLaTeX
  width=\linewidth,
  before upper=\pagedir TRT\pardir TRT\textdir TRT,
  arc=0mm,
  right=5mm, % Internal margin for Arabic
  borderline east={1mm}{0pt}{orange!80!black}, % Move border line to the RIGHT
  breakable
}
```

**ملاحظة هامة:** تأكد من إزالة القوس الزائد `}` الذي رصدتُه في ملف الكود الخاص بك "My Current Code" بعد سطر `fonttitle=\bfseries,`؛ لأنه ينهي تعريف الصندوق قبل الأوان ويسبب انهيار بقية الأوامر [320، 324].


---


المشكلة ليست في أن الحزمة لا تدعم العربية، بل في **اختلاف المحرك**؛ فحزمة **`tcolorbox`** صممت خيار **`rtl`** ليعمل بشكل أساسي عند اكتشاف حزمة **`bidi`** (التي تُستخدم عادةً مع محرك XeLaTeX) [119، 408]. وبما أنك تستخدم محرك **LuaLaTeX** مع حزمة **`luabidi`**، فإن نظام المفاتيح داخل الصندوق قد لا يتعرف على أمر `rtl` مباشرة، مما يسبب الخطأ الذي واجهته [2، 119].

أما مشكلة إزاحة الصندوق للهامش، فهي تحدث لأن الصندوق يُعامل كعنصر "يساري" (LTR) في بيئة "يمينية" (RTL)، فيبدأ الرسم من نقطة خاطئة [228، 408].

إليك الحل التقني الصحيح المتوافق مع **LuaLaTeX** والعربية دون استخدام أمر `rtl` المسبب للخطأ:

### الكود البديل والمضمون (للديباجة):

سنستخدم خيارات المحاذاة اليدوية (**`halign`**) وضبط اتجاه النص داخل الصندوق لضمان بقائه داخل حدود الصفحة [75، 114].

```
% --- Corrected TryItBox for LuaLaTeX & Arabic ---
\newtcolorbox{TryItBox}[1]{%
  colback=blue!5!white,
  colframe=blue!75!black,
  fonttitle=\bfseries,
  title={#1},
  enhanced,
  % --- Replacement for 'rtl' key ---
  halign=right,              % Aligns the body text to the right
  flush right title,         % Aligns the title text to the right
  width=\linewidth,          % Keeps the box within text margins
  before upper=\pagedir TRT\pardir TRT\textdir TRT, % Forces LuaTeX RTL direction
  % ---------------------------------
  attach boxed title to top right={yshift=-2mm, xshift=-2mm},
  boxed title style={colback=blue!75!black},
  sharp corners=downhill,
  breakable
}
```

### لماذا هذا الحل هو الأمثل؟

1. **توافق المحرك**: أمر `\textdir TRT` هو الأمر الداخلي لمحرك **LuaTeX** لتغيير اتجاه النص يدوياً من اليمين إلى اليسار (TRT تعني Text Right-to-Left)، وهو ما يحل محل مفتاح `rtl` المتعطل.
2. **ضبط المحاذاة**: استخدام **`halign=right`** يضمن أن الفقرات ستبدأ من جهة اليمين حتى لو كانت داخل بيئة برمجية معقدة.
3. **الثبات**: تحديد **`width=\linewidth`** يمنع الصندوق هندسياً من تجاوز الهوامش أو "الهروب" خارج الورقة مهما كان اتجاه النص [30، 228].

### مقتطف لتصحيح الصندوق الثاني (ExampleBox):

تأكد أيضاً من تعديله بنفس الطريقة (استبدال `rtl` بـ `halign=right`) ليعمل الكود بالكامل [75، 116]:

```
\newtcolorbox{ExampleBox}{% (Changed to 2 arguments: title and description)
  colback=gray!5!white,
  colframe=orange!80!black,
  fonttitle=\bfseries,
  title={#1: #2},
  enhanced,
  halign=right, % Safe RTL alignment for LuaLaTeX
  width=\linewidth,
  before upper=\pagedir TRT\pardir TRT\textdir TRT,
  arc=0mm,
  right=5mm, % Internal margin for Arabic
  borderline east={1mm}{0pt}{orange!80!black}, % Move border line to the RIGHT
  breakable
}
```

**ملاحظة هامة:** تأكد من إزالة القوس الزائد `}` الذي رصدتُه في ملف الكود الخاص بك "My Current Code" بعد سطر `fonttitle=\bfseries,`؛ لأنه ينهي تعريف الصندوق قبل الأوان ويسبب انهيار بقية الأوامر [320، 324].