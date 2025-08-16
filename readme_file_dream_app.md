<div align="center">

# 🏫 Dreame Admin (Education Admin Dashboard)

لوحة تحكم إدارية متكاملة لإدارة (الجامعات - الكليات - الأقسام - السنوات الأكاديمية - المصطلحات/الفصول) مع دعم متعدد اللغات (العربية / الإنجليزية)، تكامل Firebase للمصادقة وتخزين البيانات، طبقة Network عبر Dio، وإدارة حالة احترافية باستخدام BLoC + GetIt.

</div>

## 🔍 محتويات الدليل
1. نظرة عامة سريعة
2. الأهداف و السيناريو المستهدف
3. المزايا الرئيسية (Features)
4. المعمارية وطبقات المشروع
5. الهيكلية (Folder Structure) المشروحة
6. إدارة الحالة (State Management)
7. الشبكات و التعامل مع الـ API
8. Firebase و المصادقة
9. الترجمة (Localization)
10. الثيم والتصميم والخطوط
11. الموديلات (Data Models)
12. الأدوات والمساعدات (Utils & Helpers)
13. كيفية التشغيل محلياً (Setup & Run)
14. أوامر تطوير سريعة
15. جودة الكود و القواعد المتبعة
16. خارطة الطريق (Roadmap)
17. إسهامات مستقبلية
18. ملحوظات / أسئلة شائعة
19. English Quick Summary

---

## 1) نظرة عامة
تطبيق (Flutter) موجه كلوحة تحكم لإدارة كيان تعليمي: الجامعات → الكليات → الأقسام → السنوات الأكاديمية → الفصول/المصطلحات. يوفر CRUD لكل مستوى بالإضافة إلى فلاتر وواجهة تفاعلية مهيأة للتوسع (مثلاً إضافة تحليلات لاحقاً). يعتمد على:
- Firebase Authentication للمستخدمين الإداريين.
- Cloud Firestore لتخزين البيانات الهيكلية (universities, colleges, departments, ...).
- Dio + Interceptors للاتصال بـ REST API خارجي (مثلاً تسجيل الدخول APIConstant.login) عند الحاجة.
- BLoC Cubit للنمط التفاعلي وفصل منطق الأعمال عن الـ UI.
- GetIt كـ Service Locator لإدارة الاعتمادية (Dependency Injection).
- Localization (ARB) لدعم اللغتين.
- Theme مخصص + خطوط (Poppins / Cairo).

## 2) الأهداف و السيناريو المستهدف
يوفر النظام لوحة موحدة للمسؤول (Admin) لإدارة كل الكيانات التعليمية مع إمكانية التوسع لاحقاً (إضافة طلاب، مواد، تقارير، صلاحيات متقدمة، ...). الهدف الأساسي: بنية واضحة قابلة للصيانة والتوسع.

## 3) المزايا الرئيسية
- إدارة الجامعات (إضافة / تعديل / حذف / تفعيل / تعطيل + أنواع متعددة UniversityType + فلترة بالحالة والنوع).
- إدارة الكليات وربطها بالجامعة.
- إدارة الأقسام وربطها بالكلية والجامعة، مع بحث داخلي.
- إدارة السنوات الأكاديمية والفصول (قابلة للإكمال لاحقاً – بعض الملفات Placeholder حالياً مثل academic_year_model.dart).
- مصادقة Firebase (تسجيل دخول / تسجيل مستخدم Admin).
- حفظ بيانات المستخدم محلياً (Shared Preferences + CacheHelper).
- فلترة وبحث ديناميكي في الجامعات.
- بنية Theme Light/Dark جاهزة (حالياً مفعل ThemeMode.light ويمكن لاحقاً جعلها ديناميكية).
- طبقة Network مرنة عبر Dio مع Interceptors و Logging و Error Handling.
- مولد معرفات مخصص شبيه بـ Firebase (`Helper.generateFirebaseLikeId`).
- دعم تعدد اللغات (en / ar) عبر ARB + AppLocalizations.
- مكونات UI معاد استخدامها (Reusable Widgets) مثل: DefaultButton, DefaultTextFormWidget, UniversityCard, Empty/Error Screens.

## 4) المعمارية (Architecture)
النمط المتبع: (Feature-Driven + Layered)

طبقات رئيسية:
- presentation (Widgets / Screens داخل كل Feature)
- business logic (Cubits + States)
- data (Models + Firestore Collections + Network Layer)
- core (Utilities / Widgets مشتركة / Localization / Extensions / Theming)
- services (Service Locator + Network + Firebase init)

يعتمد المشروع على مبدأ Separation of Concerns وفصل المنطق عن الواجهة لضمان سهولة الاختبار والتوسعة.

## 5) الهيكلية (Folder Structure) مع شرح مختصر
```
lib/
	main.dart                  -> نقطة الدخول (تهيئة Firebase + Dio + Cache + Orientation + BlocObserver)
	dream_admin.dart           -> إعداد MaterialApp و الربط بالثيم والـ Routes والـ Localization
	config/
		app_export.dart          -> Barrel exports لتسهيل الاستيراد
		routes/                  -> تعريف المسارات الثابتة + onGenerateRoute
		style/                   -> الثيم والألوان والخطوط
	core/
		extension/               -> Extensions (Navigator, String, Int ...)
		translate/               -> ملفات ARB + التوليد
		utils/                   -> Helpers (auth, cache keys, formatter, validators, image manager ...)
		widgets/                 -> Widgets عامة (Buttons, Loaders, Empty/Error States, Inputs, ...)
	features/
		auth/                    -> مصادقة (Cubit + States + Models + Login/Register Screens)
		home/                    -> شاشة لوحة التحكم الرئيسية + عناصرها
		universities/            -> إدارة الجامعات (Cubit + Model + Screens + Widgets + Filters)
		colleges/                -> إدارة الكليات
		departments/             -> إدارة الأقسام + السنوات الأكاديمية المرتبطة
		academic_years/          -> (قيد التطوير) نماذج السنوات والفصول
	services/
		service_locator.dart     -> تهيئة GetIt و تسجيل الاعتمادية
		network/                 -> Dio Helper + Interceptors + Local Storage
		firebase/                -> firebase_options.dart (تم إنشاؤه عبر FlutterFire CLI)
```

## 6) إدارة الحالة (State Management)
- BLoC Cubit (flutter_bloc) لكل Feature (مثلاً UniversityCubit, CollegeCubit, DepartmentCubit, AuthCubit).
- كل Cubit يمتلك ملف State يحوي الحالات (Loading, Success, Error ...).
- يتم توفير الـ Cubit عبر BlocProvider في مستوى الشاشة المعنية أو باستخدام GetIt عند اللزوم.
- BlocObserver عام (AppBlocObserver) لمراقبة الانتقالات (للديباغ).

## 7) الشبكات (Networking)
- DioHelper يغلّف عمليات (GET / POST / PUT / DELETE) مع Interceptors.
- Logger مدمج (LoggerDebug) لتسهيل تتبع الطلبات.
- ErrorHandler لتجميع الأخطاء وطباعتها.
- ApiConstant يحوي baseUrl + المسارات (auth) — يمكن التوسع بإضافة Endpoints جديدة.
- حاليا البيانات الرئيسية (جامعات/كليات/أقسام) تعمل على Firestore، بينما Dio مجهز لأي تكامل REST إضافي.

## 8) Firebase & Auth
- Firebase Core + Auth + Cloud Firestore.
- تسجيل الدخول بالبريد وكلمة المرور.
- حفظ UID في CacheHelper (SharedPreferences) + تحميل بيانات المستخدم.
- إمكانية توسيع صلاحيات (Roles) لاحقاً (يوجد حقل type في ClientModel).

## 9) الترجمة (Localization)
- ملفات ARB: `app_en.arb`, `app_ar.arb` داخل `core/translate/`.
- توليد ملفات AppLocalizations عبر `flutter gen-l10n` (مُفعل عبر pubspec: generate: true).
- يمكن جعل اللغة ديناميكية لاحقاً (حالياً locale مثبتة على آخر مدخل في supportedLocales).

## 10) الثيم والخطوط (Theming & Fonts)
- تعريف ThemeData (Light / Dark) في `config/style/theme.dart`.
- ألوان منظمة في `app_color.dart`.
- خطوط Cairo + Poppins معرفة في pubspec.yaml بأسماء مخصصة (app_strings.dart يحوي الثوابت).
- تطبيق الخط في Builder داخل `dream_admin.dart` لتوحيد النصوص.

## 11) الموديلات (Models)
- UniversityModel / CollegeModel / DepartmentModel مع Equatable للمقارنة.
- يحتوي UniversityModel على `UniversityType` (enum) + امتداد لعرض اسم قابل للقراءة.
- CollegeModel و DepartmentModel يرتبطان بـ universityId و collegeId.
- AcademicYearModel / TermModel (بعضها Placeholder وسيتم استكماله).

## 12) الأدوات والمساعدات (Utils & Helpers)
- Helper: توليد معرفات، نسخ للحافظة، فتح الهاتف، إدارة اللغة.
- AuthHelper / CacheHelper: تخزين بيانات محلية.
- DateFormatter, Validators, Responsive / AppSize للمسافات المتكيفة.
- UiHelper: SnackBars / Toasts (مع fluttertoast).
- Image / Font / Share / Responsive Managers.

## 13) الإعداد والتشغيل (Setup & Run)
### المتطلبات
- Flutter SDK >= 3.8.1
- Dart (مرفق مع Flutter)
- حساب Firebase + إعداد مشروع + إضافة تطبيقات (Android / iOS / Web) + تنزيل ملفات `google-services.json` و `GoogleService-Info.plist` (موجود Android حالياً).
- (اختياري) Firebase CLI لتفعيل المزايا المتقدمة.

### خطوات الإعداد
1. استنسخ المستودع:
```powershell
git clone <repo-url> ; cd dreame_admin
```
2. ثبّت الحزم:
```powershell
flutter pub get
```
3. تأكد من ملفات Firebase (Android: `android/app/google-services.json`).
4. شغّل التطبيق:
```powershell
flutter run -d chrome
# أو: flutter run -d windows / -d emulator-5554
```
5. (اختياري) تشغيل تحليل الكود:
```powershell
flutter analyze
```
6. (اختياري) تشغيل الاختبارات:
```powershell
flutter test
```

### حسابات اختبارية
- يمكنك إنشاء مستخدم Admin عبر شاشة التسجيل أو مباشرة من Firebase Console (group: clients / users) ثم تسجيل الدخول.

## 14) أوامر تطوير سريعة
```powershell
flutter pub get               # تثبيت التبعيات
flutter clean ; flutter pub get  # إعادة تهيئة كاملة
flutter analyze               # تحليل الستاتيك
flutter test                  # تشغيل الاختبارات
flutter run -d windows        # تشغيل على ويندوز
flutter run -d chrome         # تشغيل Web
```

## 15) جودة الكود والقواعد
- التسمية: CamelCase للـ Classes, snake_case للملفات.
- كل Feature له Cubit خاص + State.
- استخدام Equatable في النماذج لتسهيل المقارنة.
- تجنب المنطق داخل Widgets قدر الإمكان (انقل للـ Cubit).
- استخدام Barrel File (`app_export.dart`) لتقليل تكرار الاستيراد.
- قابلية التوسع: أي Feature جديد يتبع نفس النمط (models / controller / view/... ).

## 16) خارطة الطريق (Roadmap)
# File Tree: dreame_admin

Generated on: 8/16/2025, 4:19:27 PM
Root path: `d:\projects\Flutter\dreame_admin`

```
├── 📁 lib/
│   ├── 📁 config/
│   │   ├── 📁 routes/
│   │   │   ├── 🔵 routers.dart
│   │   │   └── 🔵 routers_generated.dart
│   │   ├── 📁 style/
│   │   │   ├── 🔵 app_color.dart
│   │   │   ├── 🔵 hex_color.dart
│   │   │   ├── 🔵 icon_broken.dart
│   │   │   ├── 🔵 theme.dart
│   │   │   └── 🔵 theme_manager.dart
│   │   └── 🔵 app_export.dart
│   ├── 📁 core/
│   │   ├── 📁 extension/
│   │   │   ├── 🔵 extension_string.dart
│   │   │   ├── 🔵 extension_widget.dart
│   │   │   ├── 🔵 extesion_int.dart
│   │   │   └── 🔵 navigator_extension.dart
│   │   ├── 📁 translate/
│   │   │   ├── 📁 generated/
│   │   │   │   ├── 🔵 app_localizations.dart
│   │   │   │   ├── 🔵 app_localizations_ar.dart
│   │   │   │   └── 🔵 app_localizations_en.dart
│   │   │   ├── 📄 app_ar.arb
│   │   │   ├── 📄 app_en.arb
│   │   │   └── 🔵 l10n.dart
│   │   ├── 📁 utils/
│   │   │   ├── 🔵 app_size.dart
│   │   │   ├── 🔵 app_strings.dart
│   │   │   ├── 🔵 auth_helper.dart
│   │   │   ├── 🔵 content.dart
│   │   │   ├── 🔵 date_formatter.dart
│   │   │   ├── 🔵 dummy_data.dart
│   │   │   ├── 🔵 error_handler.dart
│   │   │   ├── 🔵 font_manager.dart
│   │   │   ├── 🔵 form_validator.dart
│   │   │   ├── 🔵 helper.dart
│   │   │   ├── 🔵 image_manager.dart
│   │   │   ├── 🔵 responsive_manager.dart
│   │   │   ├── 🔵 share_manager.dart
│   │   │   ├── 🔵 ui_helper.dart
│   │   │   └── 🔵 validate.dart
│   │   ├── 📁 widgets/
│   │   │   ├── 🔵 app_wrapper.dart
│   │   │   ├── 🔵 build_seation_widget.dart
│   │   │   ├── 🔵 circular_indicator_widget.dart
│   │   │   ├── 🔵 custom_empty_screen.dart
│   │   │   ├── 🔵 custome_error_screen.dart
│   │   │   ├── 🔵 custome_sliver_app_bar.dart
│   │   │   ├── 🔵 custome_state_widget.dart
│   │   │   ├── 🔵 cutom_delete_dilaog.dart
│   │   │   ├── 🔵 default_bottom.dart
│   │   │   ├── 🔵 divider_widget.dart
│   │   │   ├── 🔵 flutter_toast.dart
│   │   │   ├── 🔵 icon_button_widget.dart
│   │   │   ├── 🔵 loadin_dialog_widget.dart
│   │   │   ├── 🔵 sheet_line_widget.dart
│   │   │   ├── 🔵 shimer_widget.dart
│   │   │   ├── 🔵 textform_widget.dart
│   │   │   └── 🔵 university_dropdown_widget.dart
│   │   ├── 🔵 animation_dialog.dart
│   │   └── 🔵 animation_widget.dart
│   ├── 📁 features/
│   │   ├── 📁 auth/
│   │   │   ├── 📁 controller/
│   │   │   │   ├── 🔵 auth_cubit.dart
│   │   │   │   └── 🔵 auth_state.dart
│   │   │   ├── 📁 models/
│   │   │   │   └── 🔵 client_model.dart
│   │   │   └── 📁 view/
│   │   │       ├── 📁 screens/
│   │   │       │   ├── 🔵 login_screen.dart
│   │   │       │   └── 🔵 register_screen.dart
│   │   │       └── 📁 widget/
│   │   ├── 📁 colleges/
│   │   │   ├── 📁 controller/
│   │   │   │   ├── 🔵 college_cubit.dart
│   │   │   │   └── 🔵 college_state.dart
│   │   │   ├── 📁 models/
│   │   │   │   └── 🔵 college_model.dart
│   │   │   └── 📁 view/
│   │   │       ├── 📁 screens/
│   │   │       │   ├── 🔵 add_edit_college_screen.dart
│   │   │       │   ├── 🔵 all_colleges_screen.dart
│   │   │       │   └── 🔵 college_details_screen.dart
│   │   │       └── 📁 widgets/
│   │   │           ├── 🔵 college_card.dart
│   │   │           └── 🔵 delete_sheet_widget.dart
│   │   ├── 📁 departments/
│   │   │   ├── 📁 controller/
│   │   │   │   ├── 🔵 department_cubit.dart
│   │   │   │   └── 🔵 department_state.dart
│   │   │   ├── 📁 models/
│   │   │   │   ├── 🔵 academic_year_model.dart
│   │   │   │   ├── 🔵 department_model.dart
│   │   │   │   └── 🔵 term_model.dart
│   │   │   └── 📁 view/
│   │   │       ├── 📁 screens/
│   │   │       │   ├── 🔵 add_edit_department_screen.dart
│   │   │       │   ├── 🔵 department_details_screen.dart
│   │   │       │   ├── 🔵 departments_screen.dart
│   │   │       │   └── 🔵 term_details_screen.dart
│   │   │       └── 📁 widgets/
│   │   │           ├── 🔵 add_edit_term_sheet.dart
│   │   │           ├── 🔵 add_years_sheet.dart
│   │   │           ├── 🔵 custom_contant_widget.dart
│   │   │           ├── 🔵 delete_department_dialog.dart
│   │   │           ├── 🔵 delete_term_dialog.dart
│   │   │           ├── 🔵 delete_years_widget.dart
│   │   │           ├── 🔵 department_card_widget.dart
│   │   │           ├── 🔵 department_years_sction.dart
│   │   │           └── 🔵 view_department_details.dart
│   │   ├── 📁 error_screen/
│   │   │   ├── 📁 controller/
│   │   │   │   ├── 🔵 error_management_cubit.dart
│   │   │   │   └── 🔵 error_management_state.dart
│   │   │   ├── 📁 model/
│   │   │   └── 📁 view/
│   │   │       ├── 📁 screens/
│   │   │       │   └── 🔵 error_screen.dart
│   │   │       └── 📁 widgets/
│   │   ├── 📁 home/
│   │   │   ├── 📁 controller/
│   │   │   │   ├── 🔵 home_cubit.dart
│   │   │   │   └── 🔵 home_state.dart
│   │   │   ├── 📁 models/
│   │   │   └── 📁 view/
│   │   │       ├── 📁 screen/
│   │   │       │   └── 🔵 home_screen.dart
│   │   │       └── 📁 widget/
│   │   │           ├── 🔵 build_feature_card_widget.dart
│   │   │           └── 🔵 welcome_section_widget.dart
│   │   └── 📁 universities/
│   │       ├── 📁 controller/
│   │       │   ├── 🔵 university_cubit.dart
│   │       │   └── 🔵 university_state.dart
│   │       ├── 📁 models/
│   │       │   └── 🔵 university_model.dart
│   │       └── 📁 view/
│   │           ├── 📁 screens/
│   │           │   ├── 🔵 add_edit_university_screen.dart
│   │           │   └── 🔵 universities_screen.dart
│   │           └── 📁 widgets/
│   │               ├── 🔵 delete_university_dialog.dart
│   │               ├── 🔵 university_card.dart
│   │               └── 🔵 university_filter_widget.dart
│   ├── 📁 services/
│   │   ├── 📁 firebase/
│   │   │   ├── 🔵 error_management_service.dart
│   │   │   └── 🔵 firebase_options.dart
│   │   ├── 📁 network/
│   │   │   ├── 📁 local/
│   │   │   │   ├── 🔵 api_cache.dart
│   │   │   │   ├── 🔵 cache_helper.dart
│   │   │   │   └── 🔵 local_server.dart
│   │   │   └── 📁 remote/
│   │   │       ├── 🔵 api_constant.dart
│   │   │       ├── 🔵 dio_helper.dart
│   │   │       ├── 🔵 dio_interceptor.dart
│   │   │       └── 🔵 logger.dart
│   │   ├── 🔵 app_abserver.dart
│   │   └── 🔵 service_locator.dart
│   ├── 🔵 dream_admin.dart
│   └── 🔵 main.dart
```

---
*Generated by FileTree Pro Extension*

## 17) إسهامات مستقبلية (Contribution)
1. افتح Issue لما ستقوم به.
2. أنشئ فرع: `feature/<name>`.
3. التزم برسائل واضحة في الكوميت.
4. نفّذ التحليل والاختبارات قبل Pull Request.

## 18) أسئلة شائعة (FAQ)
س: لماذا استخدام كل من Firestore و Dio؟
ج: Firestore للمصدر الأساسي الحالي للبيانات؛ Dio مجهز لتكامل REST خارجي (مثلاً خدمات إضافية أو Auth مخصص).

س: كيف أضيف لغة جديدة؟
ج: أضف ملف ARB جديد (مثلاً app_fr.arb) ثم شغّل `flutter gen-l10n`.

س: كيف أغير نوع الثيم؟
ج: حالياً ThemeMode ثابت، يمكن إضافة Cubit لإدارته وتحديث `themeMode` في `DreamAdmin`.

س: لماذا بعض الملفات فارغة (مثل academic_year_model.dart)؟
ج: Placeholder للتطوير لاحقاً ضمن خارطة الطريق.

## 19) English Quick Summary
Dreame Admin is a modular, feature-driven Flutter admin dashboard for managing educational entities (Universities → Colleges → Departments → Academic Years / Terms). It uses Firebase (Auth + Firestore), Dio for external REST calls, BLoC (Cubit) for state management, GetIt for dependency injection, and supports multilingual UI (ar/en) with custom theming and reusable UI components. The architecture is scalable and ready for future extensions like analytics, roles/permissions, file uploads, and advanced reporting.

---

## 🛡️ ترخيص (License)
حالياً لم يُحدد ترخيص صريح. يمكن إضافة `LICENSE` لاحقاً (MIT / Apache 2.0 ...) حسب سياسة المنتج.

## ✉️ تواصل
للاستفسارات أو الاقتراحات: افتح Issue أو أرسل رسالة عبر المنصة المستضيفة للمستودع.

---
تم إنشاء هذا الملف لوصف المشروع بشكل شامل وتمهيد الطريق لتوسعات مستقبلية. ✅

