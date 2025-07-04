<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>عالم التسوق - فيديو وثيم</title>
  <style>
    body, html {
      margin: 0; padding: 0; font-family: 'Arial', sans-serif;
      background-color: var(--bg-color);
      color: var(--text-color);
      transition: background-color 0.3s, color 0.3s;
    }
    :root {
      --bg-color-light: #f5f5f5;
      --text-color-light: #333;
      --bg-color-dark: #121212;
      --text-color-dark: #eee;
    }
    body.light-theme {
      --bg-color: var(--bg-color-light);
      --text-color: var(--text-color-light);
    }
    body.dark-theme {
      --bg-color: var(--bg-color-dark);
      --text-color: var(--text-color-dark);
    }

    .container {
      max-width: 900px; margin: 20px auto; padding: 20px; background: var(--bg-color);
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      min-height: 100vh;
      box-sizing: border-box;
    }
    h1, h2 {
      text-align: center; margin-bottom: 15px;
    }
    label {
      display: block; margin-top: 10px;
    }
    select, input[type="text"], textarea, input[type="file"] {
      width: 100%; padding: 8px; margin-top: 5px; box-sizing: border-box; border-radius: 4px; border: 1px solid #ccc;
      background-color: var(--bg-color);
      color: var(--text-color);
    }
    button {
      margin-top: 15px; padding: 10px 20px; border: none; background: #6a0572; color: white; border-radius: 5px; cursor: pointer;
      transition: background 0.3s;
    }
    button:hover {
      background: #8b005d;
    }
    .product-list {
      margin-top: 30px;
      display: grid;
      grid-template-columns: repeat(auto-fill,minmax(200px,1fr));
      gap: 20px;
    }
    .product-card {
      background: var(--bg-color);
      border: 1px solid #ddd;
      border-radius: 6px; padding: 15px; box-shadow: 0 2px 5px rgba(0,0,0,0.05);
      display: flex; flex-direction: column; justify-content: space-between;
      color: var(--text-color);
    }
    .product-card img {
      max-width: 100%; height: 150px; object-fit: contain; border-radius: 4px; background: #fafafa;
    }
    .theme-toggle {
      position: fixed;
      top: 15px;
      left: 15px;
      padding: 10px 15px;
      background: #6a0572;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      z-index: 1000;
    }
    video {
      max-width: 100%;
      border-radius: 6px;
      margin-top: 10px;
      background: black;
    }

    /* زر الاعدادات */
    .settings-btn {
      position: fixed;
      top: 15px;
      right: 15px;
      background: #550055;
      color: white;
      border: none;
      border-radius: 5px;
      padding: 10px 15px;
      cursor: pointer;
      z-index: 1100;
    }
    /* نافذة الاعدادات */
    .settings-panel {
      position: fixed;
      top: 55px;
      right: 15px;
      width: 180px;
      background: var(--bg-color);
      border: 1px solid #ccc;
      box-shadow: 0 0 8px rgba(0,0,0,0.2);
      border-radius: 6px;
      padding: 15px;
      display: none;
      z-index: 1100;
      color: var(--text-color);
    }

    /* تعديل اتجاه الصفحة حسب اللغة */
    body[dir="rtl"] {
      direction: rtl;
      text-align: right;
    }
    body[dir="ltr"] {
      direction: ltr;
      text-align: left;
    }
  </style>
</head>
<body>

  <button class="theme-toggle" onclick="toggleTheme()">تبديل الثيم</button>
  <button class="settings-btn" onclick="toggleSettings()">الإعدادات ⚙️</button>

  <div class="settings-panel" id="settingsPanel">
    <label for="lang-select" id="label-lang">اختر اللغة:</label>
    <select id="lang-select" onchange="changeLanguage()">
      <option value="ar">العربية</option>
      <option value="en">English</option>
    </select>
  </div>

  <div class="container">
    <h1 data-i18n="title">عالم التسوق - فيديو وثيم</h1>

    <div class="section">
      <label for="category" data-i18n="label-category">اختر فئة المنتج:</label>
      <select id="category">
        <option value="" data-i18n="option-select">-- اختر --</option>
        <option value="ملابس" data-i18n="option-clothes">ملابس</option>
        <option value="ألعاب" data-i18n="option-toys">ألعاب</option>
        <option value="اكسسوارات" data-i18n="option-accessories">اكسسوارات</option>
        <option value="الكترونيات" data-i18n="option-electronics">قطع إلكترونية</option>
        <option value="كتب" data-i18n="option-books">كتب</option>
      </select>

      <label for="product-name" data-i18n="label-name">اسم المنتج:</label>
      <input type="text" id="product-name" placeholder="" />

      <label for="product-desc" data-i18n="label-desc">وصف المنتج:</label>
      <textarea id="product-desc" rows="4" placeholder=""></textarea>

      <label for="product-price" data-i18n="label-price">السعر (اختياري):</label>
      <input type="text" id="product-price" placeholder="" />

      <label for="product-image" data-i18n="label-image">صورة المنتج:</label>
      <input type="file" id="product-image" accept="image/*" />

      <label for="product-video" data-i18n="label-video">فيديو تعريفي للمنتج (اختياري):</label>
      <input type="file" id="product-video" accept="video/*" />

      <button onclick="addProduct()" data-i18n="btn-upload">رفع المنتج</button>
    </div>

    <div class="search-box">
      <label for="search-input" data-i18n="label-search">ابحث عن منتج:</label>
      <input type="text" id="search-input" placeholder="" oninput="renderProducts()" />
    </div>

    <h2 data-i18n="title-products">منتجاتك المرفوعة</h2>
    <div id="product-list" class="product-list"></div>
  </div>

<script>
  // ترجمة النصوص
  const translations = {
    ar: {
      title: "عالم التسوق - فيديو وثيم",
      "label-category": "اختر فئة المنتج:",
      "option-select": "-- اختر --",
      "option-clothes": "ملابس",
      "option-toys": "ألعاب",
      "option-accessories": "اكسسوارات",
      "option-electronics": "قطع إلكترونية",
      "option-books": "كتب",
      "label-name": "اسم المنتج:",
      "label-desc": "وصف المنتج:",
      "label-price": "السعر (اختياري):",
      "label-image": "صورة المنتج:",
      "label-video": "فيديو تعريفي للمنتج (اختياري):",
      "btn-upload": "رفع المنتج",
      "label-search": "ابحث عن منتج:",
      "title-products": "منتجاتك المرفوعة",
      "label-lang": "اختر اللغة:",
      "alert-fill": "يرجى ملء الحقول المطلوبة: الفئة، اسم المنتج، الوصف.",
      "alert-image": "يرجى رفع صورة للمنتج.",
      "alert-uploaded": "تم رفع المنتج بنجاح!",
      "search-no": "لا توجد منتجات مطابقة."
    },
    en: {
      title: "Shopping World - Video & Theme",
      "label-category": "Select product category:",
      "option-select": "-- Select --",
      "option-clothes": "Clothes",
      "option-toys": "Toys",
      "option-accessories": "Accessories",
      "option-electronics": "Electronics",
      "option-books": "Books",
      "label-name": "Product Name:",
      "label-desc": "Product Description:",
      "label-price": "Price (Optional):",
      "label-image": "Product Image:",
      "label-video": "Product Video (Optional):",
      "btn-upload": "Upload Product",
      "label-search": "Search for product:",
      "title-products": "Your Uploaded Products",
      "label-lang": "Select Language:",
      "alert-fill": "Please fill the required fields: category, product name, description.",
      "alert-image": "Please upload a product image.",
      "alert-uploaded": "Product uploaded successfully!",
      "search-no": "No matching products found."
    }
  };

  // تعيين اللغة
  function setLanguage(lang) {
    // تخزين اللغة
    localStorage.setItem('lang', lang);
    document.documentElement.lang = lang;

    // اتجاه الصفحة
    if (lang === 'ar') {
      document.documentElement.dir = 'rtl';
      document.body.setAttribute('dir', 'rtl');
    } else {
      document.documentElement.dir = 'ltr';
      document.body.setAttribute('dir', 'ltr');
    }

    // ترجمة كل العناصر اللي تحمل data-i18n
    document.querySelectorAll('[data-i18n]').forEach(el => {
      const key = el.getAttribute('data-i18n');
      if (translations[lang][key]) {
        if (el.tagName === 'INPUT' || el.tagName