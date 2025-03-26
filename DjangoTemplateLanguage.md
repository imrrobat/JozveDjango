خب Django Template Language (DTL) یک زبان قالب‌نویسی مخصوص فریم‌ورک جنگو است که به شما کمک می‌کند تا داده‌های پویا را در HTML نمایش دهید. این زبان دارای قابلیت‌هایی مانند متغیرها، فیلترها، تگ‌های کنترلی و ارث‌بری از قالب‌ها است.

در DTL از **علامت‌های خاصی** برای پردازش داده‌ها و کنترل نمایش استفاده می‌کنیم:  
✅ `{{ variable }}` → نمایش متغیر  
✅ `{% tag %}` → اجرای دستورات و شرط‌ها

---
برای نشون دادن متغیرها این شکلی عمل میکنن:
```html
<p>سلام {{ user_name }}!</p>
```
یه سری فیلتر وجود داره که میشه سر این متغیرها اعمال کرد بعضی از پرکاربردتریناش:
- `upper` → متن را به حروف بزرگ تبدیل می‌کند.
- `lower` → متن را به حروف کوچک تبدیل می‌کند.
- `length` → طول یک رشته یا لیست را برمی‌گرداند.
- `default` → مقدار پیش‌فرض را مشخص می‌کند.
- `date` → تاریخ را فرمت می‌کند.
- `join` → لیست را به یک رشته تبدیل می‌کند.
- `slugify` > نقطه و اینا رو برمیداره بین کلمات - میذاره
نحو استفاده از این فیلترها:
```html
<p>{{ user_name|upper }}</p>
<p>تعداد اعضا: {{ members|length }}</p>
```
لیست فیلترها توی جنگو ایناس:
https://www.w3schools.com/django/django_ref_filter.php

---

شرط ساده
```django
{% if name == 'ali' %}
  <h1>Hello</h1>
{% endif %}
```
بعدش عملگرهای مختلف هم داریم == و != و > و < و >= و <=
اما باید از دو دستور دیگه فاصله داشته باشن. 
همچنین is - not - and - or - in هم داریم.
و در نهایت else و elif هم داریم مشابه شرط ها توی پایتون هستن.

یه سری چیزا رو هم میشه چک کرد:
- `is_authenticated `
- `is_anonymous `
- `is_active`
- `is_staff `
- `is_superuser`
<div dir="rtl">
متد is_active برای اینه که وقتی جایی میخوایم کاربر رو دلیت بکنیم، دلیتش نکنیم! به جاش اینو True بذاریم. اینطوری اشیا وابسته به این یوزر بی خانمان نمیشن!
</div>

---
حلقه ساده
```django
{% for x in fruits %}
  <h1>{{ x }}</h1>
{% endfor %}
```
میشه روی یه سری چیزا حرکت کرد. 
اگه کوئری ست یا مثلا یه دیکشنری به سمت تمپلیت ارسال شده باشه ما اگر بخوایم به value اون دسترسی داشته باشیم از . استفاده میکنیم:
```django
{% for x in cars %}
  <h1>{{ x.brand }}</h1>
  <p>{{ x.model }}</p>
  <p>{{ x.year }}</p>
{% endfor %} 
```
میتونیم لیست رو برعکس پیمایش کنیم:
```django
{% for x in members reversed %}
  <h1>{{ x.id }}</h1>
  <p>
    {{ x.firstname }}
    {{ x.lastname }}
  </p>
{% endfor %} 
```
یه تگ empty داریم:
```django
<ul>
  {% for x in myobject %}
    <li>{{ x.firstname }}</li>
  {% empty %}
    <li>No members</li>
  {% endfor %}
</ul>
```
یه سری forloop.variable ها برای حلقه ها داریم که مثال استفاده اش اینطوریه:
```django
<ul>
  {% for x in fruits %}
    <li>{{ forloop.counter }}</li>
  {% endfor %}
</ul> 
```
لیست این متغیرها:

| variable            | work                                         |
| ------------------- | -------------------------------------------- |
| forloop.counter     | شمارش که از 1 شروع میشه                      |
| forloop.counter0    | شمارش که از 0 شروع میشه                      |
| forloop.first       | میتونیم یه کارایی برای اولین آیتم انجام بدیم |
| forloop.last        | میتونیم یه کارایی برای آخرین آیتم انجام بدیم |
| forloop.revcounter  | اگه پیمایش برعکس خواستیم انجام بدیم          |
| forloop.revcounter0 | پیمایش برعکس و شروع هم از صفر                |
| forloop.parentloop  | دسترسی به شمارنده حلقه والد                  |

<div dir="rtl">
مثال از forloop.first :
</div>

```django
<ul>
  {% for x in fruits %}
    <li
      {% if forloop.first %}
        style='background-color:lightblue;'
      {% endif %}
    >{{ x }}</li>
  {% endfor %}
</ul>
```
مثال از foorloop.parentloop:
مثلا اگر ما چنین چیزی رو به فرانت ارسال کنیم:
```python 
categories = [
    {"name": "الکترونیکی", "products": ["لپ‌تاپ", "موبایل", "تبلت"]},
    {"name": "پوشاک", "products": ["تی‌شرت", "شلوار", "کفش"]},
]
```
بعدش بخوایم روش پیمایش انجام بدیم:
```django
<ul>
  {% for category in categories %}
    <li>{{ category.name }}
      <ul>
        {% for product in category.products %}
          <li>
            محصول: {{ product }} | شماره دسته: {{ forloop.parentloop.counter }}
          </li>
        {% endfor %}
      </ul>
    </li>
  {% endfor %}
</ul>
```
و خروجی اینطوری میشه:
```django
<ul>
  <li>الکترونیکی
    <ul>
      <li>محصول: لپ‌تاپ | شماره دسته: 1</li>
      <li>محصول: موبایل | شماره دسته: 1</li>
      <li>محصول: تبلت | شماره دسته: 1</li>
    </ul>
  </li>
  <li>پوشاک
    <ul>
      <li>محصول: تی‌شرت | شماره دسته: 2</li>
      <li>محصول: شلوار | شماره دسته: 2</li>
      <li>محصول: کفش | شماره دسته: 2</li>
    </ul>
  </li>
</ul>
```
<hr>
### تگ‌های مهم و کاربردی دیگر
یک: autoescape
این آن و آف داره و به شکل پیشفرض روشن هستش و اگر روشن باشه:
- `<` is converted to `&lt;`
- `>` is converted to `&gt;`
- `'` is converted to `'`
- `"` is converted to `"`
- `&` is converted to `&amp;`

```django
{% autoescape on %}
  <h1>{{ heading }}</h1>
{% endautoescape %}
```

دو:comment
مثل کامنت‌ها توی برنامه‌نویسی هستش و استفاده اش اینطوریه:
```django
{% comment %}
  <h1>Greetings!</h2>
{% endcomment %}
```

سه:lorem
میتونیم یه متن برای پر کردن سایت بسازیم:
```django
{% lorem 50 w %}
```
چهار:now
برای نشون دادن زمان و تایم و اینا:
```django
{% now "Y-m-d | H:i:s" %}
```
پنج: verbatim
برای اینه که بعضی از قسمت ها رو بگیم تمپلیت رندر نکنه:
```django
{% verbatim %}
  {% for x in fruits %}
    <h1>{{ x }}</h1>
  {% endfor %}
{% endverbatim %}
```
شش:filter
نشون دادن یه بخشی به شکل دیگه ای! مثلا:
```django
{% filter upper %}
  <p>Have a great day!</p>
{% endfilter %}
```
فیلترهای مختلفی وجود داره ولی خب بازم بهتره که اونطوری که توی بخش اول گفتم استفاده کنید. اما اینم هست! البته این برای اعمال فیلتر به چند خط، کاربردی تره.
<hr>
هفت: cycle و resetcycle
اگر بخوایم بین چند مقدار هی سوییچ بکنیم از این استفاده میکنیم. مثالش:
```django
<ul>
{% for x in fruits %}
  <li style='color:{% cycle 'red' 'green' 'blue' 'pink' %}'>
    {{ x }}
  </li>
{% endfor %}
</ul>
```

```django
<ul>
  {% for x in fruits %}
    <li style='color:{% cycle 'red' 'green' 'blue' 'pink' %}'>
      {{ x }}
    </li>
    {% if x == "Banana" %}
      {% resetcycle %}
    {% endif %}
  {% endfor %}
</ul>
```
هشت: url
برای آدرس دادن به یک url هستش. استفاده ساده:
``` django
{% url 'some-url-name'%}
```
میتونیم به این url یه سری چیز میز هم بفرستیم:
``` django
{% url 'some-url-name' data1 data2 %}

in urls.py
path('/profile/<int:data1>/<int:data2>', name='some-url-name')
```
میتونیم به این دیتا ها اسم هم بدیم:
``` django
{% url 'some-url-name' a=data1 b=data2 %}

in urls.py
path('/profile/<int:a>/<int:b>', name='some-url-name')
```
نکته: اگر ترکیبی بخوایم از آرگومان‌های موقعیتی و کلیدی استفاده کنیم اولویت با موقعیتی‌هاست و بعد با کلیدیها

میتونیم یه url تعریف کنیم و بعد جاهای مختلفی استفاده کنیم ازش:
``` django
{% url 'some-url-name' arg1 arg2 as the_url %}

<a href="{{ the_url }}">I'm linking to {{ the_url }}</a>
```
نه: templatetag
برای اینه که بخوایم کدهای DTL رو نشون بدیم! اجرا نکنیم! 
```django
{% templatetag openblock %} for item in items {% templatetag closeblock %}
   {{ item }}
{% templatetag openblock %} endfor {% templatetag closeblock %}
```
لیست تمپلیت تگ ها رو نشون بده.

ده: spaceless
این تگ برای اینه که کد HTML رو فشرده تر کنیم و حجمش رو بیاریم پایین و سریع تر بشه. توی سورس فهمیده میشه.
```django
{% spaceless %}

        <ul>

          {% for x in fruits %}

            <li>{{ x }}</li>

          {% endfor %}

        </ul>
        
{% endspaceless %}
```

یازده: widthratio
مثال رو ببین:
```django
<h2>{% widthratio 4 10 100 %}%</h2>
```
مقدار فعلی: 4 
مقدار کل: 10
مقدار ماکزیمم (برای اون چیزی که نهایی میشه): 100

دوازده: regroup
اگر یه سری دیتای مرتب شده داشته باشیم، میتونیم اونو به دسته های مختلف تقسیمش کنیم و نمایشش بدیم مثلا:
```django
{% regroup cars by brand as newlist %}

{% for x in newlist %}
  <h1>{{ x.grouper }}</h1>
  {% for y in x.list %}
    <p>{{ y.model }}: {{ y.year }}</p>
  {% endfor %}
{% endfor %}
```

سیزده: ifchanged
با این میتونیم چک کنیم یه مقداری عوض شده یا نه. مثلا ببین:
```django
{% for x in mylist %}
    {% ifchanged %}
      <li>{{ x }}</li>
    {% endifchanged %}
  {% endfor %}
```
این x رو فقط وقتی عوض بشه چاپ میکنه.
این مثال بهتره:
```django
{% for x in cars %}
  {% ifchanged x.brand %}
    <h1>{{ x.brand }}:</h1>
  {% endifchanged %}
  <p>{{ x.model }}, {{ x.year }}</p>
{% endfor %}
```

چهارده: firstof
با این تگ میتونیم اولین متغیری که خالی نیست رو چاپ کنیم مثلا ببین:
```django
{% firstof user.nickname user.full_name "کاربر مهمان" %}
```
اگر نیک نیم نباشه یا فول نیم هم حتی نباشه در نهایت کاربر میهمان رو مینویسه.
پس اولین متغیری که `None` یا مقدار خالی (`""`، `[]`، `{}`، `0`، `False`) نباشه رو برامون چاپ میکنه.
