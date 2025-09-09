# Nomo Flow

تطبيق تسويقي مخصص للتجار على منصة **سلة** يضيف أدوات تفاعلية مثل:  
- كوبونات الخصم  
- التنبيهات (Email / SMS / WhatsApp / Push)  
- عداد الزوار (Live Counter)  
- جامع الإيميلات (Email Collector)  
- بيانات الزوار (Visitor Data)  
- آخر عملية شراء (Latest Conversion)  

🎯 الهدف: زيادة مبيعات المتجر وتحسين تجربة العملاء من خلال أدوات تسويقية سهلة التفعيل والإدارة.

---

## 🚀 المميزات
- **لوحة تحكم (Dashboard):** كل ميزة تظهر كبطاقة يمكن تفعيلها أو إيقافها.  
- **تكامل مع سلة (Salla Integration):** ربط OAuth 2.0 آمن مع متجر التاجر.  
- **Webhooks:** استقبال أحداث (طلب جديد، سلة متروكة، كوبون مستخدم).  
- **التقارير:** عرض وتحليل استخدام الكوبونات والتنبيهات والزوار.  
- **تخصيص:** إعدادات مرنة لكل ميزة عبر JSON.  

---

nomo-flow/
│
├─ manage.py
├─ requirements.txt
├─ .env.example
├─ README.md
│
├─ nomo_flow/                 # إعدادات Django الأساسية للمشروع
│   ├─ __init__.py
│   ├─ settings.py            # الإعدادات (التطبيقات، قاعدة البيانات، المفاتيح، اللغات، إلخ)
│   ├─ urls.py                # مسارات المشروع العامة (تجميع urls للتطبيقات)
│   ├─ wsgi.py                # تشغيل المشروع على خوادم WSGI (مثل Gunicorn)
│   └─ asgi.py                # تشغيل المشروع على ASGI (للدعم الآني / WebSockets لاحقًا)
│
├─ core/                      # لبّ المشروع وأدواته المشتركة
│   ├─ __init__.py
│   ├─ salla_client.py        # عميل التكامل مع Salla (OAuth، استدعاءات API، تحديث التوكن)
│   ├─ webhooks.py            # أدوات مساعدة للتحقق من تواقيع الويب هوكس ومعالجة الأخطاء
│   ├─ utils.py               # دوال عامة (تسجيل، توليد أكواد، تنسيقات وقت، إلخ)
│   └─ models.py              # نماذج أساسية مشتركة (إن لزم مثل Merchant/SallaToken)
│
├─ integrations/              # إعدادات الربط مع سلة
│   ├─ __init__.py
│   ├─ models.py              # Merchant, SallaToken, IntegrationSettings
│   ├─ views.py               # شاشات: ربط المتجر عبر OAuth، حالة الاتصال
│   ├─ urls.py
│   └─ admin.py
│
├─ features/                  # لوحة الميزات (Feature-based Dashboard)
│   ├─ __init__.py
│   ├─ models.py              # Feature, MerchantFeature (toggle + settings_json)
│   ├─ views.py               # عرض البطاقات، تفعيل/إيقاف، فتح نماذج Setup
│   ├─ urls.py
│   ├─ services.py            # منطق تطبيق الإعدادات على الواجهة/الخادم
│   └─ admin.py
│
├─ coupons/                   # الكوبونات (بدون مفهوم الحملات)
│   ├─ __init__.py
│   ├─ models.py              # Coupon (code, type, amount, limits …)
│   ├─ views.py               # إنشاء/تعديل كوبون + مزامنة مع Salla
│   ├─ urls.py
│   ├─ services.py            # استدعاءات إنشاء/تعديل/تعطيل الكوبونات على Salla
│   └─ admin.py
│
├─ notifications/             # التنبيهات والرسائل (Email/SMS/WhatsApp/Push)
│   ├─ __init__.py
│   ├─ models.py              # Notification (template, channel, schedule), Message (logs)
│   ├─ views.py               # بناء القوالب، الجدولة البسيطة، استعراض السجلات
│   ├─ urls.py
│   ├─ services.py            # موصلات الإرسال (Mail/SMS/WhatsApp) + Personalization
│   └─ admin.py
│
├─ visitors/                  # بيانات الزوار (Live Counter / Visitor Data)
│   ├─ __init__.py
│   ├─ models.py              # VisitorSession, PageView
│   ├─ views.py               # إحصائيات سريعة، عدّاد مباشر، استعلامات
│   ├─ urls.py
│   └─ admin.py
│
├─ events/                    # استقبال ومعالجة Webhooks من سلة
│   ├─ __init__.py
│   ├─ models.py              # Event (نوع الحدث، الحمولة، الأوقات)
│   ├─ views.py               # endpoint للاستقبال (POST /webhooks/salla)
│   ├─ urls.py
│   └─ admin.py
│
├─ attributions/              # إسناد الإيرادات (Orders ↔ Coupons/Notifications)
│   ├─ __init__.py
│   ├─ models.py              # Attribution (order, revenue_sar, used_coupon_code)
│   ├─ services.py            # منطق الربط بين الطلبات ووُسوم المصدر
│   └─ admin.py
│
├─ templates/                 # قوالب HTML (Jinja/Django Templates)
│   ├─ base.html              # القالب العام (Navbar/Sidebar/Flash messages)
│   ├─ features/              # صفحات Dashboard وبطاقات الميزات
│   ├─ coupons/               # نماذج إنشاء/تعديل كوبون
│   ├─ notifications/         # محرر القوالب + معاينة
│   ├─ visitors/              # شاشات التقارير
│   └─ integrations/          # شاشة ربط سلة وحالة الاتصال
│
├─ static/                    # ملفات الواجهة (CSS/JS/Images)
│   ├─ css/
│   ├─ js/
│   └─ images/
│
├─ scripts/                   # سكربتات مساعدة للتطوير/النشر
│   ├─ dev_seed.py            # إدخال بيانات تجريبية (Features افتراضية، تاجر تجريبي)
│   └─ export_db.sh           # مثال لتصدير قاعدة البيانات/نسخ احتياطي
│
├─ tests/                     # اختبارات وحدات/تكامل
│   ├─ __init__.py
│   ├─ test_features.py
│   ├─ test_coupons.py
│   └─ test_notifications.py
│
└─ docs/                      # وثائق المشروع
    ├─ ERD.dbml              # مخطط dbdiagram.io
    ├─ UX-flow.md            # رحلة المستخدم المختصرة
    └─ API-notes.md          # ملاحظات التكامل مع Salla (endpoints ونطاقات الصلاحيات)
