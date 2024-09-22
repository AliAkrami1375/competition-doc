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





## 2. مدل‌های پایگاه داده (MongoDB Models)

### مدل کاربر (User Model)
```typescript
import { Schema, Document } from 'mongoose';

export interface User extends Document {
  username: string;
  email: string;
  password: string;
  role: string; // 'user' or 'admin'
  eventsParticipated: string[]; // لیست ایونت‌های شرکت‌کرده
  trades: any[]; // لیست تریدهای انجام‌شده
  createdAt: Date;
}

export const UserSchema = new Schema({
  username: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  role: { type: String, enum: ['user', 'admin'], default: 'user' },
  eventsParticipated: [{ type: Schema.Types.ObjectId, ref: 'Event' }],
  trades: [{
    tradeId: String,
    result: String, // مثلا 'win' یا 'loss'
    amount: Number,
    timestamp: Date
  }],
  createdAt: { type: Date, default: Date.now }
});
```

### مدل ادمین (Admin Model)
```typescript
import { Schema, Document } from 'mongoose';

export interface Admin extends Document {
  username: string;
  email: string;
  password: string;
  role: string; // 'admin' only
  managedEvents: string[]; // لیست ایونت‌های مدیریت‌شده
  createdAt: Date;
}

export const AdminSchema = new Schema({
  username: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  role: { type: String, enum: ['admin'], default: 'admin' },
  managedEvents: [{ type: Schema.Types.ObjectId, ref: 'Event' }],
  createdAt: { type: Date, default: Date.now }
});
```

### مدل ایونت (Event Model)
```typescript
import { Schema, Document } from 'mongoose';

export interface Event extends Document {
  eventName: string;
  startDate: Date;
  endDate: Date;
  participants: string[]; // لیست کاربران شرکت‌کننده
  createdAt: Date;
}

export const EventSchema = new Schema({
  eventName: { type: String, required: true },
  startDate: { type: Date, required: true },
  endDate: { type: Date, required: true },
  participants: [{ type: Schema.Types.ObjectId, ref: 'User' }],
  createdAt: { type: Date, default: Date.now }
});
```

## 3. کنترلرها (Controllers)

### کنترلر کاربران (User Controller)
```typescript
import { Controller, Get, Post, Body, Param, Delete } from '@nestjs/common';
import { UsersService } from './users.service';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  getAllUsers() {
    return this.usersService.findAll();
  }

  @Get(':id')
  getUserById(@Param('id') id: string) {
    return this.usersService.findById(id);
  }

  @Post()
  createUser(@Body() createUserDto: any) {
    return this.usersService.create(createUserDto);
  }

  @Delete(':id')
  deleteUser(@Param('id') id: string) {
    return this.usersService.delete(id);
  }
}
```

### کنترلر ادمین (Admin Controller)
```typescript
import { Controller, Get, Post, Body, Param, Delete } from '@nestjs/common';
import { AdminsService } from './admins.service';

@Controller('admins')
export class AdminsController {
  constructor(private readonly adminsService: AdminsService) {}

  @Get()
  getAllAdmins() {
    return this.adminsService.findAll();
  }

  @Get(':id')
  getAdminById(@Param('id') id: string) {
    return this.adminsService.findById(id);
  }

  @Post()
  createAdmin(@Body() createAdminDto: any) {
    return this.adminsService.create(createAdminDto);
  }

  @Delete(':id')
  deleteAdmin(@Param('id') id: string) {
    return this.adminsService.delete(id);
  }
}
```

### کنترلر ایونت‌ها (Event Controller)
```typescript
import { Controller, Get, Post, Body, Param, Delete } from '@nestjs/common';
import { EventsService } from './events.service';

@Controller('events')
export class EventsController {
  constructor(private readonly eventsService: EventsService) {}

  @Get()
  getAllEvents() {
    return this.eventsService.findAll();
  }

  @Get(':id')
  getEventById(@Param('id') id: string) {
    return this.eventsService.findById(id);
  }

  @Post()
  createEvent(@Body() createEventDto: any) {
    return this.eventsService.create(createEventDto);
  }

  @Delete(':id')
  deleteEvent(@Param('id') id: string) {
    return this.eventsService.delete(id);
  }
}
```

## 4. سرویس‌ها (Services)

### سرویس کاربران (User Service)
```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { User } from './user.model';

@Injectable()
export class UsersService {
  constructor(@InjectModel('User') private readonly userModel: Model<User>) {}

  async findAll() {
    return this.userModel.find().exec();
  }

  async findById(id: string) {
    return this.userModel.findById(id).exec();
  }

  async create(userDto: any) {
    const newUser = new this.userModel(userDto);
    return newUser.save();
  }

  async delete(id: string) {
    return this.userModel.findByIdAndDelete(id).exec();
  }
}
```

### سرویس ادمین‌ها (Admin Service)
```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Admin } from './admin.model';

@Injectable()
export class AdminsService {
  constructor(@InjectModel('Admin') private readonly adminModel: Model<Admin>) {}

  async findAll() {
    return this.adminModel.find().exec();
  }

  async findById(id: string) {
    return this.adminModel.findById(id).exec();
  }

  async create(adminDto: any) {
    const newAdmin = new this.adminModel(adminDto);
    return newAdmin.save();
  }

  async delete(id: string) {
    return this.adminModel.findByIdAndDelete(id).exec();
  }
}
```

