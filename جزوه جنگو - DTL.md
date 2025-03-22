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
مثال از forloop.first :
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
