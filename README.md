# CONVERT_TO_NOTHING
### توضیحات کد Pine Script برای GitHub README

این کد یک اندیکاتور در **Pine Script** (زبان برنامه‌نویسی پلتفرم TradingView) است که دو عملیات اصلی را روی یک عدد ورودی انجام می‌دهد:

1. **حذف صفرها**: تمام صفرهای موجود در عدد ورودی حذف می‌شوند.
2. **معکوس کردن ارقام**: ارقام عدد باقی‌مانده پس از حذف صفرها، معکوس می‌شوند.

علاوه بر این، تعداد ارقام عدد ورودی و عدد خروجی نیز محاسبه و نمایش داده می‌شود. خروجی به صورت یک جدول در گوشه سمت راست بالای چارت نمایش داده می‌شود.

---

### بخش‌های اصلی کد

#### 1. **تعریف اندیکاتور**
```pinescript
//@version=6
indicator('Remove Zeros and Reverse Digits with Digit Count', overlay = true)
```
- `//@version=6`: نسخه Pine Script استفاده شده (نسخه ۶).
- `indicator`: تعریف یک اندیکاتور جدید با نام `'Remove Zeros and Reverse Digits with Digit Count'`.
- `overlay = true`: اندیکاتور روی چارت اصلی نمایش داده می‌شود.

---

#### 2. **تابع شمارش ارقام**
```pinescript
f_count_digits(input_int) =>
    input_int == 0 ? 1 : math.floor(math.log10(math.abs(input_int))) + 1
```
- این تابع تعداد ارقام یک عدد صحیح را محاسبه می‌کند.
- اگر عدد صفر باشد، تعداد ارقام ۱ در نظر گرفته می‌شود.
- برای اعداد دیگر، از تابع `math.log10` برای محاسبه تعداد ارقام استفاده می‌شود.

---

#### 3. **تابع حذف صفرها و معکوس کردن ارقام**
```pinescript
f_remove_zeros_and_reverse(input_int) =>
    input_str = str.tostring(input_int)
    len = str.length(input_str)

    // Remove zeros
    str_no_zeros = ''
    i = 0
    while i < len
        curr_char = str.substring(input_str, i, i + 1)
        if curr_char != '0'
            str_no_zeros := str_no_zeros + curr_char
            str_no_zeros
        i := i + 1
        i

    // Reverse the string
    len_no_zeros = str.length(str_no_zeros)
    reversed = ''
    j = len_no_zeros - 1
    while j >= 0
        reversed := reversed + str.substring(str_no_zeros, j, j + 1)
        j := j - 1
        j

    result = str.tonumber(reversed)
    [result]
```
- این تابع یک عدد صحیح را دریافت می‌کند و دو کار انجام می‌دهد:
  1. **حذف صفرها**: با استفاده از یک حلقه `while`، تمام صفرهای عدد حذف می‌شوند.
  2. **معکوس کردن ارقام**: با استفاده از یک حلقه `while` دیگر، ارقام باقی‌مانده معکوس می‌شوند.
- در نهایت، نتیجه به صورت یک عدد بازگردانده می‌شود.

---

#### 4. **دریافت ورودی و محاسبات**
```pinescript
input_int = math.floor(input.source(close, title = "source"))
[result] = f_remove_zeros_and_reverse(input_int)
```
- `input_int`: عدد ورودی از کاربر دریافت می‌شود. در اینجا از قیمت بسته شدن (`close`) استفاده شده است.
- `math.floor`: عدد به نزدیک‌ترین عدد صحیح گرد می‌شود.
- `f_remove_zeros_and_reverse`: تابعی که صفرها را حذف و ارقام را معکوس می‌کند.

---

#### 5. **محاسبه تعداد ارقام**
```pinescript
input_digits = f_count_digits(input_int)
output_digits = f_count_digits(result)
```
- `input_digits`: تعداد ارقام عدد ورودی.
- `output_digits`: تعداد ارقام عدد خروجی.

---

#### 6. **ایجاد و به‌روزرسانی جدول**
```pinescript
var table data_table = table.new(position.top_right, 3, 4, bgcolor = color.navy, border_width = 1)

if barstate.islast
    table.cell(data_table, 0, 0, 'Description', bgcolor = color.gray, text_color = color.white)
    table.cell(data_table, 1, 0, 'Value', bgcolor = color.gray, text_color = color.white)
    table.cell(data_table, 2, 0, 'Digits', bgcolor = color.gray, text_color = color.white)

    table.cell(data_table, 0, 1, 'Input Number', text_color = color.white)
    table.cell(data_table, 1, 1, str.tostring(input_int), text_color = color.white)
    table.cell(data_table, 2, 1, str.tostring(input_digits), text_color = color.white)

    table.cell(data_table, 0, 2, 'Output Number', text_color = color.white)
    table.cell(data_table, 1, 2, 'Result' + '→' + str.tostring(result), text_color = color.white)
    table.cell(data_table, 2, 2, str.tostring(output_digits), text_color = color.white)

    table.cell(data_table, 0, 3, 'Description', text_color = color.white)
    table.cell(data_table, 1, 3, 'Removes zeros and reverses digits', text_color = color.white)
```
- `table.new`: یک جدول جدید ایجاد می‌شود که در گوشه سمت راست بالای چارت نمایش داده می‌شود.
- `table.cell`: اطلاعات مربوط به عدد ورودی، عدد خروجی و تعداد ارقام در جدول نمایش داده می‌شوند.
- `barstate.islast`: این شرط اطمینان می‌دهد که جدول فقط در آخرین کندل به‌روزرسانی شود.

---

### خروجی
این اندیکاتور یک جدول نمایش می‌دهد که شامل موارد زیر است:
1. **عدد ورودی** و تعداد ارقام آن.
2. **عدد خروجی** (پس از حذف صفرها و معکوس کردن ارقام) و تعداد ارقام آن.
3. **توضیحات** درباره عملکرد اندیکاتور.

---

### نحوه استفاده
1. کد را در **TradingView** کپی و پیست کنید.
2. اندیکاتور را به چارت اضافه کنید.
3. جدول خروجی در گوشه سمت راست بالای چارت نمایش داده می‌شود.

---

### مثال
اگر عدد ورودی `102030` باشد:
1. پس از حذف صفرها: `123`.
2. پس از معکوس کردن ارقام: `321`.
3. تعداد ارقام عدد ورودی: `6`.
4. تعداد ارقام عدد خروجی: `3`.

این اطلاعات در جدول نمایش داده می‌شوند.
