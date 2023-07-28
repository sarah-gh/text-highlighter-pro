# text-highlighter-pro

# ویرایشگر متن Vue با امکان انتخاب و رنگ‌آمیزی بخش‌های متن

این کد یک کامپوننت Vue است که یک ویرایشگر متن ساده با قابلیت انتخاب و رنگ‌آمیزی بخش‌های مختلف متن ایجاد می‌کند.

## نصب و راه‌اندازی

1. نصب وابستگی‌ها با استفاده از npm:


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

2. وارد کردن کامپوننت در پروژه:

   ```javascript
   import TextEditor from './TextEditor.vue';
   
   export default {
       components: {
           TextEditor,
       },
   };
   ```

3. استفاده از کامپوننت `TextEditor` در قالب HTML:

   ```html
   <template>
       <div>
           <TextEditor />
       </div>
   </template>
   ```

## راه‌اندازی کتابخانه‌ها

کد از دو کتابخانه `rangy` و `bootstrap-vue` استفاده می‌کند. از `rangy` برای کار با محدوده‌های متنی (Range) و ایجاد بخش‌های رنگی استفاده شده است. همچنین، `bootstrap-vue` برای نمایش توضیحات به صورت tooltip استفاده می‌شود.

## اجزاء و عملکرد

کامپوننت `TextEditor` دارای اجزاء زیر است:

1. `handleSelection()`: تابعی که با فراخوانی آن می‌توان بخش‌های انتخابی متن را تشخیص داد و با فراخوانی تابع `removeInternalTags()`، بخش‌های داخلی را حذف می‌کند.

2. `removeInternalTags(range)`: تابعی که بخش‌های داخلی انتخابی را حذف می‌کند تا از اشکال در انتخاب متن جلوگیری شود.

3. `deleteSelectedText(id)`: تابعی که با فراخوانی آن بخش انتخاب شده با ID مشخص شده حذف می‌شود.

4. `reset()`: تابعی که با فراخوانی آن همه بخش‌های انتخاب شده و تغییر رنگ آن‌ها را بازنشانی می‌کند.

5. `changeText()`: تابعی که با فراخوانی آن وضعیت ویرایش متن را فعال می‌کند.

6. `applyEditedText()`: تابعی که با فراخوانی آن تغییرات اعمال شده در متن ویرایش شده به متن اصلی اعمال می‌شود.

7. و ...

## نکته مهم

از این کد، برای ایجاد یک ویرایشگر متنی ساده با امکان انتخاب و رنگ‌آمیزی بخش‌های مختلف متن می‌توانید استفاده کنید. اگر نیازمندی‌ها و قابلیت‌های بیشتری دارید، می‌توانید این کد را به محضر خود تطبیق دهید.
