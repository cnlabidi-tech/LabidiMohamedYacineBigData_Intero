# 🧪 TP08: محاكاة MapReduce باستخدام Python  
## 📊 المثال الأول: Word Count

## 🔗 رابط الكود (Google Colab)

https://colab.research.google.com/drive/1tXy3WPq7M9Hw_6wB4CDVkbdgEP1KSC3K?usp=sharing
---

## 📌 المقدمة
في هذا العمل التطبيقي قمنا بمحاكاة نموذج **MapReduce** باستخدام لغة Python على منصة Google Colab، دون الحاجة إلى تثبيت أنظمة معالجة البيانات الضخمة مثل Hadoop أو Spark.

يهدف هذا المثال إلى فهم طريقة عمل MapReduce من خلال تطبيق بسيط وهو **عدّ تكرار الكلمات** داخل ملف نصي.

---

## 🎯 أهداف التجربة
- فهم مبدأ MapReduce  
- التعرف على المراحل الثلاث: Map → Shuffle → Reduce  
- تطبيق خوارزمية Word Count  
- التعامل مع البيانات على شكل (مفتاح، قيمة)

---

## 📂 وصف البيانات
تم إنشاء ملف نصي باسم `data.txt` يحتوي على الجمل التالية:


spark makes big data easy
big data needs spark power
python is great for data processing


---

## ⚙️ مراحل تنفيذ MapReduce

### 1️⃣ مرحلة Mapping (التحويل)
في هذه المرحلة:
- يتم قراءة كل سطر من الملف
- تقسيم السطر إلى كلمات
- تحويل كل كلمة إلى زوج (كلمة، 1)

🔹 مثال:

"big data" → [('big',1), ('data',1)]


💡 الفكرة: كل كلمة تُعتبر ظهورًا واحدًا.

---

### 2️⃣ مرحلة Shuffle (التجميع)
في هذه المرحلة:
- يتم تجميع القيم حسب الكلمة (المفتاح)
- كل كلمة تُجمع مع القيم الخاصة بها

🔹 مثال:

('data',1), ('data',1), ('data',1)
↓
'data': [1,1,1]


💡 الهدف: وضع جميع القيم الخاصة بنفس الكلمة في مجموعة واحدة.

---

### 3️⃣ مرحلة Reduce (الاختزال)
في هذه المرحلة:
- يتم جمع القيم لكل كلمة
- نحصل على عدد مرات ظهور كل كلمة

🔹 مثال:

'data': [1,1,1] → 3


---

## 🔄 التنفيذ الكامل
تم استخدام دالة `mapreduce()` لتنفيذ جميع المراحل:

1. قراءة الملف  
2. تطبيق مرحلة Mapping  
3. تطبيق مرحلة Shuffle  
4. تطبيق مرحلة Reduce  

---

## 💻 الكود المستخدم

```python
# إنشاء الملف
with open("data.txt", "w") as f:
    f.write("""spark makes big data easy
big data needs spark power
python is great for data processing""")

# Mapper
def mapper(line):
    words = line.strip().split()
    return [(word, 1) for word in words]

# Shuffle
from collections import defaultdict
def shuffle(mapped_data):
    grouped = defaultdict(list)
    for key, value in mapped_data:
        grouped[key].append(value)
    return grouped

# Reducer
def reducer(grouped_data):
    reduced = {}
    for key, values in grouped_data.items():
        reduced[key] = sum(values)
    return reduced

# MapReduce
def mapreduce(filename):
    with open(filename, 'r') as f:
        lines = f.readlines()

    mapped = []
    for line in lines:
        mapped.extend(mapper(line))

    grouped = shuffle(mapped)
    reduced = reducer(grouped)

    return reduced

# تشغيل البرنامج
results = mapreduce("data.txt")

for word, count in sorted(results.items()):
    print(word, ":", count)
📊 النتائج المتحصل عليها
big: 2
data: 3
easy: 1
for: 1
great: 1
is: 1
makes: 1
needs: 1
power: 1
processing: 1
python: 1
spark: 2
🧠 تحليل النتائج
الكلمة data هي الأكثر تكرارًا (3 مرات)
الكلمات big و spark ظهرت مرتين
باقي الكلمات ظهرت مرة واحدة
النتائج صحيحة وتعكس عمل الخوارزمية بدقة
💡 الخلاصة

من خلال هذا التطبيق تمكنا من:

فهم مبدأ MapReduce
تطبيق مثال عملي (Word Count)
ملاحظة كيفية تحويل البيانات إلى شكل (مفتاح، قيمة)

هذا النموذج هو الأساس في أنظمة معالجة البيانات الضخمة مثل Hadoop وSpark.