المساحة التي تقصدها هي **الديباجة (The Preamble)**، وهي الجزء الممتد من بداية الملف حتى أمر `\begin{document}`.

لتحقيق طلبك بضبط **هامش التجليد (Binding Offset)** ليكون متبادلاً (مرة يميناً ومرة يساراً)، يجب تفعيل خيار **`twoside`** في صنف المستند، واستخدام حزمة **`geometry`** مع معامل **`bindingoffset`**.

إليك الحزم والأوامر جاهزة للنسخ مباشرة في "الديباجة":

```
% --- 1. تعريف نوع المستند مع تفعيل الطباعة على الوجهين لتبادل الهوامش ---
\documentclass[12pt, a4paper, twoside]{article}

% --- 2. ضبط الهوامش وهامش التجليد ---
% المعامل bindingoffset يضيف مساحة إضافية للتجليد في الجهة الداخلية (inner)
% في الصفحات الفردية تكون يميناً (للعربي) وفي الزوجية تكون يساراً
\usepackage[inner=2.5cm, outer=2cm, top=2.5cm, bottom=2.5cm, bindingoffset=1cm]{geometry}

% --- 3. حزم اللغة العربية والخطوط (XeLaTeX) ---
\usepackage{fontspec}
\usepackage{polyglossia}
\usepackage{luabidi} % لضمان اتجاه النصوص الصحيح

\setmainlanguage{arabic}
\setotherlanguage{english}

% تعريف الخط العربي (يجب أن يكون مثبتاً على جهازك)
\newfontfamily\arabicfont[Script=Arabic, Scale=1.1]{Amiri}
\newfontfamily\englishfont{Latin Modern Roman}

% --- 4. حزم إضافية احترافية (من مشاريعك السابقة) ---
\usepackage{amsmath, amsfonts, amssymb} % للرموز الرياضية
\usepackage{graphicx} % لإدراج الصور
\usepackage{float} % لتثبيت أماكن الصور والجداول بـ [H]
\usepackage{xcolor} % لإدارة الألوان
\usepackage{fancyhdr} % لتخصيص الرؤوس والتذييلات
\usepackage{hyperref} % للروابط التفاعلية

% اختصارات سريعة كنت تستخدمها
\newcommand{\ar}{\textarabic}
\newcommand{\en}{\textenglish}

% --- بداية المستند ---
\begin{document}
% محتواك يبدأ هنا
\end{document}
```

### شرح الأوامر المتعلقة بالهوامش:

- الـ **`twoside`**: يخبر LaTeX أن المستند سيُطبع على الوجهين، مما يجعل الهوامش "مرآة" لبعضها (Symmetrical layout).
- الـ **`bindingoffset=1cm`**: يترك مساحة 1 سم إضافية في الجهة المخصصة للتجليد فقط، وهي تتغير تلقائياً بين اليمين واليسار حسب رقم الصفحة (فردي أم زوجي).
- الـ **`inner` و `outer`**: بدلاً من `left` و `right`؛ نستخدم `inner` للهامش الداخلي (جهة التجليد) و `outer` للهامش الخارجي.

**نصيحة إضافية:** بما أنك تكتب بالعربية، فإن LaTeX في وضع `twoside` سيعتبر الهامش الداخلي (جهة التجليد) في الصفحات الفردية على جهة اليمين، وهو التنسيق الصحيح للكتب العربية.