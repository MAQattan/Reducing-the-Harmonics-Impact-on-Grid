لإصلاح هذه المشكلات وضبط التنسيق ليصبح احترافياً ومتبادلاً كما طلبت، سنستخدم حزمة **`fancyhdr`** مع تخصيص المحددات (`RO`, `LE`) لضمان ظهور العناصر في الهوامش الخارجية (البعيدة عن التجليد) وفي أسفل الصفحة.

إليك الكود المحدث للديباجة (Preamble) مع الحلول لكل النقاط:

### 1. الكود البرمجي المحدث (للديباجة)

```tex
\usepackage{fancyhdr}
\pagestyle{fancy}
\fancyhf{} % Clear all default headers and footers

% --- 1. Remove horizontal rules ---
\renewcommand{\headrulewidth}{0pt} % Remove header line
\renewcommand{\footrulewidth}{0pt} % Ensure footer line is 0pt

% --- 2. Customize Section Marks ---
% This makes \leftmark only contain the section title without the number
\renewcommand{\sectionmark}[5]{\markboth{#1}{}}

% --- 3. Configure the Footer (Bottom of the page) ---
% RO: Right side of Odd pages (Outer margin for Arabic books)
% We place: Page Number | Section Title
\fancyfoot[RO]{\fontsize{12}{14}\selectfont \thepage \ \ | \ \ \leftmark}

% LE: Left side of Even pages (Outer margin)
% We place: "القسم" + Section Number | Page Number
\fancyfoot[LE]{\fontsize{12}{14}\selectfont \textarabic{القسم} \thesection \ \ | \ \ \thepage}

% --- 4. Alignment Fix ---
% To ensure text stays at the margins and doesn't drift to center
\fancyhfoffset[RO,LE]{0pt}
```

### 2. شرح الحلول التقنية بناءً على طلبك

- **تبادل الترقيم واسم القسم (الخروج من الوسط إلى الهامش):** المشكلة كانت في استخدام محددات داخلية؛ لذا استخدمنا **`RO`** (الجهة اليمنى للصفحات الفردية) و **`LE`** (الجهة اليسرى للصفحات الزوجية). في الكتب العربية، هذه هي الهوامش الخارجية، مما يضمن ظهور الترقيم بعيداً عن "الوسط" أو منطقة التجليد [271، 306].
- **الترقيم بالأسفل وبدون خط:** قمنا بنقل الأوامر من `\fancyhead` إلى **`\fancyfoot`** لإظهارها في الأسفل، واستخدمنا `\renewcommand{\headrulewidth}{0pt}` لإلغاء الخط العرضي تماماً [302، 312].
- **ظهور اسم القسم مقابل رقم القسم:**
    - في الصفحات الفردية (`RO`)، استخدمنا **`\leftmark`** لجلب اسم القسم.
    - في الصفحات الزوجية (`LE`)، استخدمنا **`\thesection`** مع كلمة "القسم" لجلب الرقم فقط [144، 145].
- **منع ظهور الترقيم في صفحات بداية الأقسام:** لتحقيق ذلك، يجب عليك وضع الأمر التالي **مباشرة بعد** أمر القسم في متن المستند [294، 34]:
    
    ```
    \section{مقدمة}
    \thispagestyle{empty} % This removes header/footer from the current page only
    ```
    
    سيؤدي هذا إلى جعل صفحة "المقدمة" (ذات الخط الكبير) نظيفة تماماً من أي ترقيم أو نصوص إضافية في الأسفل، بينما يستمر التنسيق تلقائياً في الصفحات التالية.

### ملاحظة حول "القسم" أو "الفصل":

إذا كنت تستخدم صنف المستند `book` بدلاً من `article` وتريد كتابة "الفصل"، يمكنك استبدال كلمة `القسم` بـ `\chaptername` أو كتابتها يدوياً كما في الكود أعلاه [27، 287].

**هل التباعد بين الرقم والرمز "|" مناسب الآن، أم تفضل زيادة المسافة بينهما؟**