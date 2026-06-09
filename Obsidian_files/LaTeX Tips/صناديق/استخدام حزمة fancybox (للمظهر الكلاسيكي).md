
إذا كنت تريد صناديق بلمسات جمالية مثل الظلال أو الحواف البيضوية دون تعقيد `tcolorbox`؛ فإن حزمة **`fancybox`** هي الخيار الأمثل المذكور في مصادرك [17، 245].

**أضف في الديباجة:**

```tex
\usepackage{fancybox} % For shadow, oval, and double boxes
```

**أمثلة للاستخدام:**

- **صندوق بظل (Shadow Box):**
    
    ```tex
    \shadowbox{\begin{minipage}{0.8\linewidth}
    هذا النص يظهر داخل صندوق بظل احترافي وبسيط.
    \end{minipage}}
    ```
    
- **صندوق بحواف دائرية (Oval Box):**
    
    ```tex
    \ovalbox{\begin{minipage}{0.8\linewidth}
    تنسيق هادئ وبسيط لشرح فكرة ما.
    \end{minipage}}
    ```

- صندوق مزدوج (Double boxes):
```tex
\doublebox{\begin{minipage}{0.8\linewidth}
هذا الصندوق يستخدم إطار مزدوج لإبراز المعلومات المهمة.
\end{minipage}}
```

---
### تطوير `fancybox` بخلفية ملونة وتوسيط العنوان

حزمة `fancybox` لا تدعم الألوان افتراضياً، لذا سنقوم بـ "تغليف" النص بـ `\colorbox` داخل الإطار البيضاوي.

**تأكد من وجود الحزم التالي في الديباجة:**

```tex
\usepackage{fancybox, xcolor}
```

**استخدم في المتن**:

```tex
\begin{center}
\ovalbox{%  % Frame from fancybox
\colorbox{gray!15}{%  % Silver background inside the frame
\begin{minipage}{0.8\linewidth}
    \vspace{2mm}
    \begin{center} \textbf{عنوان في الوسط} \end{center} % Center title manually
    \raggedleft % Ensure Arabic text stays right [6]
    هذا النص الآن لديه خلفية فضية داخل إطار fancybox الكلاسيكي.
    \vspace{2mm}
\end{minipage}%
}}
\end{center}
```

- **توسيط العنوان:** استخدام بيئة `center` داخل `minipage` هو الحل الصحيح لضمان استقلال العنوان عن محاذاة النص الأساسي.