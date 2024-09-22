# Let's create a markdown file containing the required documentation

markdown_content = """
# Competition Project - Admin Panel Backend Documentation

## 1. کلیات معماری
- **فریمورک:** Nest.js
- **دیتابیس:** MongoDB
- **احراز هویت:** JWT (JSON Web Token) برای سطوح دسترسی
- **ساختار ماژولار:** هر بخش باید به صورت ماژول جداگانه پیاده‌سازی شود

## 2. داشبورد (Dashboard)
داشبورد باید شامل نمایش اطلاعات کلی و آماری باشد.

### API‌های مورد نیاز:
- `GET /dashboard/summary`: برای دریافت خلاصه آمار شامل تعداد کاربران، ایونت‌ها، و وضعیت تریدها
- `GET /dashboard/events`: دریافت لیست ایونت‌های فعال و وضعیت آن‌ها

## 3. مدیریت کاربران (Users Management)
بخش مدیریت کاربران باید شامل امکاناتی مانند اضافه کردن، حذف و مشاهده کاربران باشد.

### API‌های مورد نیاز:
- `GET /users`: دریافت لیست کاربران
- `GET /users/:id`: دریافت جزئیات یک کاربر خاص
- `POST /users/create`: ایجاد کاربر جدید
- `PUT /users/update/:id`: بروزرسانی اطلاعات یک کاربر
- `DELETE /users/delete/:id`: حذف کاربر

## 4. مدیریت ادمین‌ها (Admins Management)
این بخش باید مدیریت کامل ادمین‌ها را فراهم کند.

### API‌های مورد نیاز:
- `GET /admins`: دریافت لیست ادمین‌ها
- `GET /admins/:id`: دریافت جزئیات یک ادمین
- `POST /admins/create`: ایجاد ادمین جدید
- `PUT /admins/update/:id`: بروزرسانی اطلاعات یک ادمین
- `DELETE /admins/delete/:id`: حذف ادمین

## 5. مدیریت ایونت‌ها (Events Management)
این بخش برای مدیریت ایونت‌ها شامل ایجاد، ویرایش و حذف ایونت‌ها است.

### API‌های مورد نیاز:
- `GET /events`: دریافت لیست ایونت‌ها
- `GET /events/:id`: دریافت جزئیات یک ایونت
- `POST /events/create`: ایجاد ایونت جدید
- `PUT /events/update/:id`: بروزرسانی اطلاعات یک ایونت
- `DELETE /events/delete/:id`: حذف ایونت

## 6. احراز هویت و امنیت (Authentication & Security)
برای مدیریت امنیت و سطوح دسترسی باید از JWT استفاده شود.

### API‌های مورد نیاز:
- `POST /auth/login`: ورود و دریافت توکن
- `POST /auth/logout`: خروج کاربر
- `POST /auth/refresh`: تجدید توکن

## 7. آنالیز کاربران (Users Analytics)
این بخش باید شامل گزارشات فعالیت کاربران و مشارکت در ایونت‌ها باشد.

### API‌های مورد نیاز:
- `GET /analytics/users`: دریافت گزارش فعالیت همه کاربران
- `GET /analytics/user/:id`: دریافت گزارش فعالیت یک کاربر خاص
"""

# Saving the markdown content to a file
file_path = "/mnt/data/competition_admin_panel_documentation.md"
with open(file_path, "w") as file:
    file.write(markdown_content)

file_path
