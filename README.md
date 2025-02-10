<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>أداة التصميم والتخطيط الذكي للمنشآت – النظام المتطور</title>
  <!-- استدعاء الخطوط وأيقونات Font Awesome -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
  <!-- خط "Cairo" -->
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap">
  <!-- مكتبة AOS لتأثيرات الحركة -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aos@2.3.4/dist/aos.css">
  <!-- مكتبة Chart.js لعرض الرسوم البيانية -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <!-- مكتبة jsPDF لتصدير التقارير إلى PDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <!-- مكتبة SheetJS لتصدير النتائج إلى Excel -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <!-- مكتبة Three.js لعرض التصميم ثلاثي الأبعاد -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <!-- OrbitControls من Three.js -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/examples/js/controls/OrbitControls.js"></script>
  <style>
    /* إعدادات الخطوط والألوان الأساسية */
    :root {
      --primary-color: #8B4513;
      --secondary-color: #1abc9c;
      --accent-color: #FFD700;
      --text-color: #333;
      --bg-gradient: linear-gradient(45deg, #654321, #8B4513, #A0522D);
    }
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      scroll-behavior: smooth;
    }
    body {
      font-family: 'Cairo', sans-serif;
      background: var(--bg-gradient);
      background-size: 400% 400%;
      min-height: 100vh;
      direction: rtl;
      color: var(--text-color);
      transition: background 0.5s, color 0.5s;
    }
    body.light-mode {
      --primary-color: #4a4a4a;
      --secondary-color: #3498db;
      --accent-color: #e67e22;
      --text-color: #333;
      --bg-gradient: linear-gradient(45deg, #f0f0f0, #fff);
      background: var(--bg-gradient);
    }
    @keyframes gradientBG {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    /* شاشة التحميل */
    .loader {
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background: var(--bg-gradient);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 3000;
    }
    .loader::after {
      content: "";
      width: 50px;
      height: 50px;
      border: 5px solid var(--secondary-color);
      border-top-color: var(--accent-color);
      border-radius: 50%;
      animation: spin 1s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }
    
    /* زر تغيير النظام */
    #modeToggle {
      position: fixed;
      top: 20px;
      left: 20px;
      background: var(--secondary-color);
      color: #fff;
      border: none;
      padding: 10px 20px;
      border-radius: 5px;
      z-index: 2100;
      cursor: pointer;
      transition: background 0.3s, transform 0.3s;
    }
    #modeToggle:hover { background: #16a085; transform: scale(1.05); }
    
    /* تنسيق الحاوية الرئيسية */
    .container {
      max-width: 1200px;
      margin: 30px auto;
      padding: 30px;
      background-color: #fff;
      border-radius: 8px;
      box-shadow: 0 0 15px rgba(0,0,0,0.1);
      direction: rtl;
    }
    h1 {
      text-align: center;
      color: var(--primary-color);
      margin-bottom: 20px;
      font-size: 2.5em;
    }
    h2 {
      color: var(--primary-color);
      margin-bottom: 10px;
      border-bottom: 2px solid var(--primary-color);
      padding-bottom: 10px;
      font-size: 1.8em;
    }
    h3 {
      color: var(--secondary-color);
      margin-bottom: 10px;
      font-size: 1.6em;
    }
    label {
      display: block;
      margin-bottom: 8px;
      font-weight: bold;
    }
    input, select, button {
      width: 100%;
      padding: 10px;
      margin-bottom: 15px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 16px;
    }
    button {
      background-color: var(--primary-color);
      color: #fff;
      cursor: pointer;
      border: none;
      transition: background 0.3s, transform 0.3s;
    }
    button:hover { background-color: var(--accent-color); transform: scale(1.02); }
    .section {
      margin-bottom: 40px;
      padding-bottom: 20px;
      border-bottom: 1px solid #eee;
    }
    .result {
      margin-top: 20px;
      padding: 20px;
      background-color: #ecf0f1;
      border-radius: 4px;
      text-align: center;
      font-size: 1.2em;
    }
    .chart-container { margin-top: 30px; }
    .export-buttons { margin-top: 20px; text-align: center; }
    .export-buttons button { margin: 5px; }
    .reset-button { background-color: #d9534f; margin-bottom: 20px; }
    .reset-button:hover { background-color: #c9302c; }
    
    /* قسم التضخم */
    .inflation-section {
      margin-top: 20px;
      padding: 10px;
      background-color: #fdfdfd;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    .inflation-section label { font-weight: bold; }
    
    /* قسم أداة التصميم والتخطيط الذكي للمنشآت */
    .design-section {
      margin-top: 40px;
      padding: 20px;
      background-color: #eef9f1;
      border: 1px solid #c2e0c6;
      border-radius: 8px;
    }
    .design-section h2 { color: var(--primary-color); margin-bottom: 10px; }
    .design-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 20px;
      margin-top: 20px;
    }
    .design-card {
      background-color: #fff;
      border: 1px solid #ddd;
      border-radius: 8px;
      overflow: hidden;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      transition: transform 0.3s;
    }
    .design-card:hover { transform: scale(1.03); }
    .design-card .card-header {
      background: var(--secondary-color);
      color: #fff;
      padding: 10px;
      text-align: center;
      font-weight: bold;
    }
    .design-card .card-content {
      padding: 15px;
    }
    .design-card .card-content p {
      font-size: 1em;
      color: #555;
      line-height: 1.4;
    }
    .design-card img {
      width: 100%;
      height: auto;
      display: block;
    }
    
    /* قسم عرض التصميم ثنائي وثلاثي الأبعاد */
    .view-section {
      margin-top: 40px;
      padding: 20px;
      background-color: #f0f7f4;
      border: 1px solid #b2d8c7;
      border-radius: 8px;
    }
    .view-section h2 { color: var(--primary-color); margin-bottom: 10px; }
    .view-section .toggle-buttons {
      text-align: center;
      margin-bottom: 15px;
    }
    .view-section .toggle-buttons button {
      margin: 0 5px;
      padding: 10px 20px;
    }
    #view2DContainer, #view3DContainer {
      margin-top: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      overflow: hidden;
      height: 500px;
    }
    #view2D {
      width: 100%;
      height: 100%;
      display: block;
    }
    
    /* قسم تحليل السيناريوهات */
    #scenarioResults {
      margin-top: 20px;
      padding: 15px;
      background-color: #f7f7f7;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
    #scenarioComparison {
      margin-top: 20px;
    }
    
    /* قسم النتائج والتقارير النهائية */
    .report-section {
      margin-top: 40px;
      padding: 20px;
      background-color: #f3f3f3;
      border: 1px solid #ccc;
      border-radius: 8px;
    }
    .report-section h2 { color: var(--primary-color); margin-bottom: 10px; }
    .report-section canvas { display: block; margin: 0 auto; }
    
    /* الأقسام التعليمية والتوضيحية */
    .info-section {
      margin-top: 40px;
      padding: 20px;
      background-color: #f9f9f9;
      border-radius: 8px;
      border: 1px solid #ddd;
      font-size: 1.1em;
    }
    .info-section h2 { color: var(--primary-color); margin-bottom: 10px; }
    .info-section p { margin-bottom: 10px; color: #555; line-height: 1.6; }
    .info-section ul { list-style-type: disc; margin-right: 20px; color: #555; }
    .info-section ul li { margin-bottom: 5px; }
    
    /* زر العودة للأعلى */
    #backToTop {
      position: fixed;
      bottom: 40px;
      right: 20px;
      background: var(--secondary-color);
      color: #fff;
      border: none;
      padding: 10px;
      border-radius: 50%;
      font-size: 1.5em;
      cursor: pointer;
      display: none;
      z-index: 2000;
      transition: background 0.3s;
    }
    #backToTop:hover { background: #16a085; }
    
    /* تأثير حركي للأزرار العامة */
    .btn-animate {
      transition: transform 0.3s, box-shadow 0.3s;
    }
    .btn-animate:hover {
      transform: scale(1.05);
      box-shadow: 0 5px 15px rgba(0,0,0,0.2);
    }
    
    /* تنسيق جدول السيناريوهات */
    #scenarioComparison table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 15px;
    }
    #scenarioComparison th, #scenarioComparison td {
      border: 1px solid #ccc;
      padding: 10px;
      text-align: center;
    }
    #scenarioComparison th { background-color: var(--primary-color); color: #fff; }
  </style>
</head>
<body>
  <!-- شاشة التحميل -->
  <div id="loader" class="loader"></div>
  
  <div class="container">
    <h1>أداة التصميم والتخطيط الذكي للمنشآت – النظام المتطور</h1>
    <p style="text-align: center; color: #555;">
      نظام شامل لتوليد مخطط بناء أولي ذكي وفق أعلى المعايير العالمية مع دعم 2D و3D وتحليل السيناريوهات والتكلفة.
    </p>
    
    <!-- نموذج إدخال معطيات المشروع -->
    <div class="section" id="dataSection" data-aos="fade-up">
      <h2>معطيات المشروع</h2>
      <p style="color: #555; font-size: 1.1em;">
        أدخل المعلومات الأساسية للمشروع:  
        اسم المشروع، مساحة الأرض، مساحة البناء، عدد الأدوار، عدد غرف النوم، الحمامات، المطابخ، وغرف المعيشة؛
        مع تحديد تقسيم الأرض (مثلاً: 60% مبنى، 20% حديقة، 10% كراج، 10% مدخل) واتجاه المبنى والنمط التصميمي.
      </p>
      <label for="projectName" title="مثال: مشروع إنشاء فيلا سكنية">اسم المشروع:</label>
      <input type="text" id="projectName" placeholder="أدخل اسم المشروع">
      
      <label for="projectArea" title="أدخل مساحة الأرض بالمتر المربع">مساحة الأرض (م²):</label>
      <input type="number" id="projectArea" placeholder="أدخل مساحة الأرض">
      
      <label for="buildingArea" title="أدخل المساحة التي سيتم بناء المبنى عليها (م²)">مساحة البناء (م²):</label>
      <input type="number" id="buildingArea" placeholder="أدخل مساحة البناء">
      
      <label for="numFloors" title="أدخل عدد الأدوار المطلوبة">عدد الأدوار:</label>
      <input type="number" id="numFloors" placeholder="أدخل عدد الأدوار">
      
      <label for="numRooms" title="أدخل عدد غرف النوم">عدد غرف النوم:</label>
      <input type="number" id="numRooms" placeholder="أدخل عدد غرف النوم">
      
      <label for="numBathrooms" title="أدخل عدد الحمامات">عدد الحمامات:</label>
      <input type="number" id="numBathrooms" placeholder="أدخل عدد الحمامات">
      
      <label for="numKitchens" title="أدخل عدد المطابخ">عدد المطابخ:</label>
      <input type="number" id="numKitchens" placeholder="أدخل عدد المطابخ">
      
      <label for="numLivingRooms" title="أدخل عدد غرف المعيشة">عدد غرف المعيشة:</label>
      <input type="number" id="numLivingRooms" placeholder="أدخل عدد غرف المعيشة">
      
      <label for="landDivision" title="أدخل تفاصيل تقسيم الأرض (مثلاً: 60% مبنى، 20% حديقة، 10% كراج، 10% مدخل)">تفاصيل تقسيم الأرض:</label>
      <input type="text" id="landDivision" placeholder="مثلاً: 60% مبنى، 20% حديقة، 10% كراج، 10% مدخل">
      
      <label for="buildingOrientation" title="اختر الاتجاه الرئيسي للمبنى">اتجاه المبنى:</label>
      <select id="buildingOrientation">
        <option value="north">شمال</option>
        <option value="south">جنوب</option>
        <option value="east">شرق</option>
        <option value="west">غرب</option>
      </select>
      
      <label for="designStyle" title="اختر النمط التصميمي المطلوب">النمط التصميمي:</label>
      <select id="designStyle">
        <option value="european">أوروبي</option>
        <option value="american">أمريكي</option>
        <option value="asian">آسيوي</option>
        <option value="eastern">شرقي</option>
        <option value="modern">حديث</option>
        <option value="traditional">تقليدي</option>
      </select>
      
      <!-- معطيات إضافية للتكلفة والتضخم -->
      <label for="totalCost" title="أدخل التكلفة الإجمالية للمشروع (اختياري)">التكلفة الإجمالية للمشروع:</label>
      <input type="number" id="totalCost" placeholder="أدخل التكلفة الإجمالية">
      
      <label for="currency" title="اختر العملة المستخدمة">اختر العملة:</label>
      <select id="currency">
        <option value="YER" selected>الريال اليمني (YER)</option>
        <option value="USD">الدولار الأمريكي (USD)</option>
      </select>
      
      <label for="exchangeRate" title="أدخل سعر الصرف الحالي بين الريال والدولار">سعر الصرف (ريال/دولار):</label>
      <input type="number" id="exchangeRate" placeholder="أدخل سعر الصرف" value="250">
      <button onclick="updateExchangeRate()" class="btn-animate" style="margin-bottom:15px;">
        <i class="fas fa-sync-alt"></i> تحديث سعر الصرف
      </button>
      
      <!-- قسم التضخم (اختياري) -->
      <div class="inflation-section" data-aos="fade-up">
        <label for="useInflation" title="حدد إذا كنت تريد احتساب التضخم السنوي">احتساب التضخم السنوي:</label>
        <input type="checkbox" id="useInflation">
        <label for="inflationRate" title="أدخل معدل التضخم السنوي (%)">معدل التضخم السنوي (%):</label>
        <input type="number" id="inflationRate" placeholder="مثال: 3" value="3">
      </div>
      
      <button onclick="generateFloorPlan()" class="btn-animate">
        <i class="fas fa-drafting-compass"></i> إنشاء المخطط الأولي
      </button>
      <button class="reset-button btn-animate" onclick="resetDesignTool()">
        <i class="fas fa-undo"></i> إعادة تعيين المعطيات
      </button>
    </div>
    
    <!-- قسم عرض المخطط الأولي (بطاقات التصميم) -->
    <div class="section design-section" id="designResults" data-aos="fade-up">
      <h2>المخططات الأولية المقترحة</h2>
      <div id="floorPlanPreview" class="design-grid">
        <!-- بطاقات التصميم ستظهر هنا -->
      </div>
      <div class="export-buttons">
        <button onclick="printResult()" class="btn-animate">
          <i class="fas fa-print"></i> طباعة المخطط
        </button>
        <button onclick="exportPDF()" class="btn-animate">
          <i class="fas fa-file-pdf"></i> مشاركة المخطط
        </button>
        <button onclick="exportExcel()" class="btn-animate">
          <i class="fas fa-file-excel"></i> تصدير إلى Excel
        </button>
      </div>
    </div>
    
    <!-- قسم عرض التصميم بنمط 2D و3D -->
    <div class="section view-section" id="viewSection" data-aos="fade-up">
      <h2>عرض التصميم</h2>
      <div class="toggle-buttons">
        <button class="btn-animate" onclick="show2DView()">عرض 2D</button>
        <button class="btn-animate" onclick="show3DView()">عرض 3D</button>
      </div>
      <div id="view2DContainer" style="display: none;">
        <canvas id="view2D" style="width: 100%; height: 500px; border: 1px solid #ccc;"></canvas>
      </div>
      <div id="view3DContainer" style="display: none; width: 100%; height: 500px;"></div>
    </div>
    
    <!-- قسم تحليل السيناريوهات ومقارنتها -->
    <div class="section" id="scenarioSection" data-aos="fade-up">
      <h2>تحليل السيناريوهات ومقارنتها</h2>
      <p style="color: #555;">جرب معطيات مختلفة واحفظ السيناريوهات للمقارنة بين المخططات المقترحة.</p>
      <div style="display: flex; gap: 10px; margin-bottom: 20px;">
        <button onclick="calculateScenario('سيناريو 1')" class="btn-animate">
          <i class="fas fa-percentage"></i> سيناريو 1
        </button>
        <button onclick="calculateScenario('سيناريو 2')" class="btn-animate">
          <i class="fas fa-percentage"></i> سيناريو 2
        </button>
        <button onclick="calculateScenario('سيناريو 3')" class="btn-animate">
          <i class="fas fa-percentage"></i> سيناريو 3
        </button>
      </div>
      <div id="scenarioResults"></div>
      <button onclick="saveScenario()" class="btn-animate" style="margin-top: 10px;">
        <i class="fas fa-save"></i> حفظ السيناريو الحالي
      </button>
      <div id="scenarioComparison"></div>
    </div>
    
    <!-- قسم النتائج والتقارير النهائية مع رسم بياني (Chart.js) -->
    <div class="section report-section" id="resultSection" data-aos="fade-up">
      <h2>نتائج التحليل والتكلفة</h2>
      <p id="costAnalysis"></p>
      <canvas id="costChart" style="max-width: 800px; margin: 20px auto;"></canvas>
    </div>
    
    <!-- الأقسام التعليمية والتوضيحية -->
    <div class="info-section" data-aos="fade-up">
      <h2>دليل المستخدم</h2>
      <p>توضح هذه الوثيقة كيفية استخدام النظام خطوة بخطوة:</p>
      <ul>
        <li>أدخل معطيات المشروع الأساسية مثل مساحة الأرض، مساحة البناء، عدد الأدوار، غرف النوم، الحمامات، المطابخ، وغرف المعيشة.</li>
        <li>حدد تفاصيل تقسيم الأرض (مثلاً: 60% مبنى، 20% حديقة، 10% كراج، 10% مدخل) واتجاه المبنى.</li>
        <li>اختر النمط التصميمي المطلوب (أوروبي، أمريكي، آسيوي، شرقي، حديث، تقليدي) وفقاً لتفضيلاتك.</li>
        <li>اضغط على "إنشاء المخطط الأولي" لتوليد النماذج التصميمية الأولية تلقائيًا.</li>
        <li>تظهر المخططات على شكل بطاقات تعرض تفاصيل التصميم مع تقسيم الأرض واتجاه المبنى.</li>
        <li>استخدم قسم "عرض التصميم" للتبديل بين العرض الثنائي الأبعاد (2D) والثلاثي الأبعاد (3D).</li>
        <li>يمكنك تجربة سيناريوهات مختلفة وحفظها للمقارنة باستخدام وحدة تحليل السيناريوهات.</li>
        <li>يتم حساب تحليل التكلفة (على سبيل المثال، التكلفة لكل متر مربع) وعرضه مع رسم بياني تفاعلي.</li>
        <li>يمكنك تصدير النتائج إلى PDF أو Excel أو طباعتها مباشرةً.</li>
      </ul>
    </div>
    
    <div class="info-section" data-aos="fade-up">
      <h2>المعايير الهندسية والتصميمية العالمية</h2>
      <p>يستند التصميم إلى معايير هندسية عالمية تشمل:</p>
      <ul>
        <li>توزيع المساحات بدقة لضمان استغلال أمثل للأرض.</li>
        <li>اختيار اتجاه الواجهة الرئيسية لتحقيق أفضل إضاءة وتهوية طبيعية.</li>
        <li>اعتماد أنماط تصميمية مثل الأوروبي، الأمريكي، الآسيوي، الشرقي، الحديث، والتقليدي وفقًا لأفضل الممارسات.</li>
        <li>تطبيق معايير السلامة والجودة باستخدام مواد بناء معتمدة وتصاميم آمنة.</li>
        <li>تحليل التكلفة والتشغيل لتحقيق جدوى اقتصادية متكاملة.</li>
      </ul>
      <p>يساهم التصميم الدقيق في تحسين جودة المشروع وخفض التكاليف وضمان بيئة معيشية وعملية متميزة.</p>
    </div>
    
    <div class="info-section" data-aos="fade-up">
      <h2>خطوات التصميم والتخطيط الصحيحة</h2>
      <ul>
        <li>جمع وتحليل معطيات المشروع (الموقع، المساحات، احتياجات المستخدم، الميزانية).</li>
        <li>تحديد توزيع المساحات وفق معايير هندسية دقيقة (المبنى، الحديقة، الكراج، المدخل، الشوارع).</li>
        <li>اختيار النمط التصميمي المناسب (أوروبي، أمريكي، آسيوي، شرقي، حديث، تقليدي).</li>
        <li>توليد المخطط الأولي باستخدام محرك تصميم ذكي يعتمد على مبادئ التصميم البارامتري.</li>
        <li>تحليل التكلفة والتشغيل مع مراجعة معايير السلامة والجودة.</li>
        <li>مقارنة السيناريوهات المختلفة لاتخاذ القرار الأنسب بناءً على النتائج.</li>
      </ul>
      <p>يضمن التخطيط الدقيق تحقيق توازن بين الجمالية والوظيفية مع تقليل التكاليف وزيادة كفاءة المشروع.</p>
    </div>
    
  </div>
  
  <!-- زر العودة للأعلى -->
  <button id="backToTop" title="العودة للأعلى" aria-label="العودة للأعلى">↑</button>
  
  <!-- الفوتر (يظهر مرة واحدة فقط) -->
  <footer style="background: #333; color: #ccc; padding: 20px; text-align: center; direction: rtl; margin-top: 40px;">
    <p style="margin-bottom: 10px;">© 2024 منصة الاتحاد العام للمقاولين اليمنيين - جميع الحقوق محفوظة</p>
    <p>
      <a href="https://guyc-yemen.com/" style="color: var(--accent-color); text-decoration: none;">الموقع الرسمي</a> |
      <a href="mailto:info@guyc-ye.com" style="color: var(--accent-color); text-decoration: none;">البريد الإلكتروني</a> |
      <a href="tel:01450553" style="color: var(--accent-color); text-decoration: none;">01-450553</a>
    </p>
    <p style="margin-top: 10px;">
      <a href="#" style="color: var(--accent-color); margin: 0 5px; text-decoration: none;">
        <i class="fab fa-facebook"></i>
      </a>
      <a href="#" style="color: var(--accent-color); margin: 0 5px; text-decoration: none;">
        <i class="fab fa-twitter"></i>
      </a>
      <a href="#" style="color: var(--accent-color); margin: 0 5px; text-decoration: none;">
        <i class="fab fa-youtube"></i>
      </a>
      <a href="#" style="color: var(--accent-color); margin: 0 5px; text-decoration: none;">
        <i class="fab fa-whatsapp"></i>
      </a>
    </p>
  </footer>
  
  <!-- استدعاء مكتبة AOS -->
  <script src="https://cdn.jsdelivr.net/npm/aos@2.3.4/dist/aos.js"></script>
  <script>
    window.addEventListener('load', () => {
      AOS.init();
      document.getElementById('loader').style.display = 'none';
      loadInputs();
      updateCostChart();
    });
  </script>
  
  <!-- دوال النظام -->
  <script>
    // حفظ واستعادة المدخلات باستخدام localStorage
    function saveInputs() {
      const inputs = {
        projectName: document.getElementById('projectName').value,
        landArea: document.getElementById('projectArea').value,
        buildingArea: document.getElementById('buildingArea').value,
        numFloors: document.getElementById('numFloors').value,
        numRooms: document.getElementById('numRooms').value,
        numBathrooms: document.getElementById('numBathrooms').value,
        numKitchens: document.getElementById('numKitchens').value,
        numLivingRooms: document.getElementById('numLivingRooms').value,
        landDivision: document.getElementById('landDivision').value,
        buildingOrientation: document.getElementById('buildingOrientation').value,
        designStyle: document.getElementById('designStyle').value,
        totalCost: document.getElementById('totalCost').value,
        exchangeRate: document.getElementById('exchangeRate').value,
        buildingType: document.getElementById('buildingType').value,
        buildingUsage: document.getElementById('buildingUsage').value,
        financingType: document.getElementById('financingType').value,
        financingPercent: document.getElementById('financingPercent').value,
        annualRevenue: document.getElementById('annualRevenue').value,
        annualExpenses: document.getElementById('annualExpenses').value,
        interestRate: document.getElementById('interestRate').value,
        projectDuration: document.getElementById('projectDuration').value,
        useInflation: document.getElementById('useInflation') ? document.getElementById('useInflation').checked : false,
        inflationRate: document.getElementById('inflationRate') ? document.getElementById('inflationRate').value : ""
      };
      localStorage.setItem('designInputs', JSON.stringify(inputs));
    }
    function loadInputs() {
      const saved = localStorage.getItem('designInputs');
      if (saved) {
        const inputs = JSON.parse(saved);
        document.getElementById('projectName').value = inputs.projectName || "";
        document.getElementById('projectArea').value = inputs.landArea || "";
        document.getElementById('buildingArea').value = inputs.buildingArea || "";
        document.getElementById('numFloors').value = inputs.numFloors || "";
        document.getElementById('numRooms').value = inputs.numRooms || "";
        document.getElementById('numBathrooms').value = inputs.numBathrooms || "";
        document.getElementById('numKitchens').value = inputs.numKitchens || "";
        document.getElementById('numLivingRooms').value = inputs.numLivingRooms || "";
        document.getElementById('landDivision').value = inputs.landDivision || "";
        document.getElementById('buildingOrientation').value = inputs.buildingOrientation || "north";
        document.getElementById('designStyle').value = inputs.designStyle || "european";
        document.getElementById('totalCost').value = inputs.totalCost || "";
        document.getElementById('exchangeRate').value = inputs.exchangeRate || "250";
        document.getElementById('buildingType').value = inputs.buildingType || "residential";
        document.getElementById('buildingUsage').value = inputs.buildingUsage || "residential";
        document.getElementById('financingType').value = inputs.financingType || "bankLoan";
        document.getElementById('financingPercent').value = inputs.financingPercent || "30";
        document.getElementById('annualRevenue').value = inputs.annualRevenue || "";
        document.getElementById('annualExpenses').value = inputs.annualExpenses || "";
        document.getElementById('interestRate').value = inputs.interestRate || "8";
        document.getElementById('projectDuration').value = inputs.projectDuration || "10";
        if(document.getElementById('useInflation')) {
          document.getElementById('useInflation').checked = inputs.useInflation || false;
          document.getElementById('inflationRate').value = inputs.inflationRate || "3";
        }
      }
    }
    
    // زر تحديث سعر الصرف من API
    function updateExchangeRate() {
      fetch("https://api.exchangerate-api.com/v4/latest/USD")
        .then(response => response.json())
        .then(data => {
          if(data && data.rates && data.rates["YER"]) {
            const rate = data.rates["YER"];
            document.getElementById("exchangeRate").value = rate;
            alert("تم تحديث سعر الصرف: " + rate);
            saveInputs();
          } else {
            alert("تعذر الحصول على سعر الصرف.");
          }
        })
        .catch(err => alert("فشل تحديث سعر الصرف: " + err));
    }
    
    // تحديث النتائج عند تغيير المدخلات باستخدام debounce
    const inputFields = document.querySelectorAll('#dataSection input, #dataSection select');
    let debounceTimer;
    inputFields.forEach(field => {
      field.addEventListener('input', () => {
        clearTimeout(debounceTimer);
        debounceTimer = setTimeout(() => {
          generateFloorPlan();
          saveInputs();
        }, 500);
      });
    });
    
    // دالة لحساب التحليل المالي (تكلفة البناء لكل م² كمثال)
    function calculateCostAnalysis() {
      let totalCost = parseFloat(document.getElementById('totalCost').value) || 0;
      let buildingArea = parseFloat(document.getElementById('buildingArea').value) || 0;
      if (buildingArea > 0) {
        let costPerM2 = totalCost / buildingArea;
        return "التكلفة لكل م²: " + costPerM2.toFixed(2) + " " + document.getElementById('currency').value;
      }
      return "لم يتم إدخال مساحة البناء أو التكلفة.";
    }
    
    // دالة توليد المخطط الأولي وتوليد بطاقات التصميم وتحديث تحليل التكلفة
    function generateFloorPlan() {
      const projectName = document.getElementById('projectName').value || "المشروع غير محدد";
      const landArea = parseFloat(document.getElementById('projectArea').value) || 0;
      const buildingArea = parseFloat(document.getElementById('buildingArea').value) || 0;
      const numFloors = parseInt(document.getElementById('numFloors').value) || 1;
      const numRooms = parseInt(document.getElementById('numRooms').value) || 0;
      const numBathrooms = parseInt(document.getElementById('numBathrooms').value) || 0;
      const numKitchens = parseInt(document.getElementById('numKitchens').value) || 0;
      const numLivingRooms = parseInt(document.getElementById('numLivingRooms').value) || 0;
      const landDivision = document.getElementById('landDivision').value || "60% مبنى، 20% حديقة، 10% كراج، 10% مدخل";
      const buildingOrientation = document.getElementById('buildingOrientation').value;
      const designStyle = document.getElementById('designStyle').value;
      
      // بناء مصفوفة من النماذج التصميمية وفق النمط المختار
      let proposals = [];
      if(designStyle === "european") {
        proposals = [
          {
            title: "تصميم أوروبي كلاسيكي",
            description: "تصميم متوازن مع واجهات فاخرة وحدائق واسعة؛ يناسب المشاريع الراقية مع توزيع دقيق للمساحات.",
            image: "https://via.placeholder.com/400x250?text=أوروبي+كلاسيكي"
          },
          {
            title: "تصميم أوروبي معاصر",
            description: "يعتمد على توزيع مفتوح ومساحات داخلية متناسقة مع لمسات عصرية.",
            image: "https://via.placeholder.com/400x250?text=أوروبي+معاصر"
          }
        ];
      } else if(designStyle === "american") {
        proposals = [
          {
            title: "تصميم أمريكي عصري",
            description: "مساحات واسعة ونوافذ كبيرة لإضاءة طبيعية مثالية، مع مدخل مميز وكراج منفصل.",
            image: "https://via.placeholder.com/400x250?text=أمريكي+عصري"
          },
          {
            title: "تصميم أمريكي عملي",
            description: "استغلال أمثل للمساحة مع توزيع وظيفي متكامل يلبي الاحتياجات العملية.",
            image: "https://via.placeholder.com/400x250?text=أمريكي+عملي"
          }
        ];
      } else if(designStyle === "asian") {
        proposals = [
          {
            title: "تصميم آسيوي هادئ",
            description: "يعكس الانسجام مع الطبيعة بخطوط بسيطة وتوزيع مريح يُبرز الهدوء والراحة.",
            image: "https://via.placeholder.com/400x250?text=آسيوي+هادئ"
          },
          {
            title: "تصميم آسيوي معاصر",
            description: "يجمع بين البساطة والعصرية لتحقيق توزيع داخلي متناسق مع عناصر تصميم أنيقة.",
            image: "https://via.placeholder.com/400x250?text=آسيوي+معاصر"
          }
        ];
      } else if(designStyle === "eastern") {
        proposals = [
          {
            title: "تصميم شرقي تقليدي",
            description: "يعتمد على التفاصيل الدقيقة والزخارف الفنية مع توزيع كلاسيكي يراعي التقاليد.",
            image: "https://via.placeholder.com/400x250?text=شرقي+تقليدي"
          },
          {
            title: "تصميم شرقي حديث",
            description: "يجمع بين العناصر التقليدية والحديثة لتحقيق نموذج متوازن يناسب الاحتياجات المعاصرة.",
            image: "https://via.placeholder.com/400x250?text=شرقي+حديث"
          }
        ];
      } else if(designStyle === "modern") {
        proposals = [
          {
            title: "تصميم حديث مبتكر",
            description: "يعتمد على تقنيات التصميم الذكي مع واجهات زجاجية وتوزيع ديناميكي للمساحات لخلق بيئة داخلية متطورة.",
            image: "https://via.placeholder.com/400x250?text=حديث+مبتكر"
          },
          {
            title: "تصميم مودرن عملي",
            description: "يركز على تحقيق أفضل استغلال للمساحات مع دمج تقنيات ذكية للتحكم في البيئة الداخلية.",
            image: "https://via.placeholder.com/400x250?text=مودرن+عملي"
          }
        ];
      } else if(designStyle === "traditional") {
        proposals = [
          {
            title: "تصميم تقليدي كلاسيكي",
            description: "يعتمد على الأساليب التقليدية مع استخدام مواد طبيعية وتوزيع فني يراعي التقاليد المحلية.",
            image: "https://via.placeholder.com/400x250?text=تقليدي+كلاسيكي"
          },
          {
            title: "تصميم تقليدي معاصر",
            description: "يجمع بين الأصالة والحداثة لتقديم نموذج متوازن يلبي الاحتياجات المحلية.",
            image: "https://via.placeholder.com/400x250?text=تقليدي+معاصر"
          }
        ];
      }
      
      // ضمان وجود 6 نماذج على الأقل
      let allProposals = proposals;
      if(allProposals.length < 6) {
        while(allProposals.length < 6) {
          allProposals = allProposals.concat(proposals);
        }
        allProposals = allProposals.slice(0, 6);
      }
      
      // توليد بطاقات المخطط الأولي مع تفاصيل إضافية (تقسيم الأرض واتجاه المبنى)
      let html = "";
      allProposals.forEach((proposal, index) => {
        html += `
          <div class="design-card btn-animate" data-aos="fade-up">
            <div class="card-header">${proposal.title}</div>
            <div class="card-content">
              <p>${proposal.description}</p>
              <p><strong>تقسيم الأرض:</strong> ${document.getElementById('landDivision').value || "60% مبنى، 20% حديقة، 10% كراج، 10% مدخل"}</p>
              <p><strong>اتجاه المبنى:</strong> ${getOrientationLabel(document.getElementById('buildingOrientation').value)}</p>
            </div>
            <img src="${proposal.image}" alt="${proposal.title}">
          </div>
        `;
      });
      document.getElementById("floorPlanPreview").innerHTML = html;
      
      // تحديث قسم نتائج التحليل المالي
      const costText = calculateCostAnalysis();
      document.getElementById("costAnalysis").innerText = costText;
      updateCostChart();
    }
    
    // دالة مساعدة لتحويل اتجاه المبنى إلى نص عربي
    function getOrientationLabel(orientation) {
      switch (orientation) {
        case "north": return "شمال";
        case "south": return "جنوب";
        case "east":  return "شرق";
        case "west":  return "غرب";
        default: return orientation;
      }
    }
    
    // دالة إعادة تعيين أداة التصميم
    function resetDesignTool() {
      document.getElementById('projectName').value = "";
      document.getElementById('projectArea').value = "";
      document.getElementById('buildingArea').value = "";
      document.getElementById('numFloors').value = "";
      document.getElementById('numRooms').value = "";
      document.getElementById('numBathrooms').value = "";
      document.getElementById('numKitchens').value = "";
      document.getElementById('numLivingRooms').value = "";
      document.getElementById('landDivision').value = "";
      document.getElementById('buildingOrientation').selectedIndex = 0;
      document.getElementById('designStyle').selectedIndex = 0;
      document.getElementById('totalCost').value = "";
      document.getElementById('exchangeRate').value = "250";
      document.getElementById('buildingType').selectedIndex = 0;
      document.getElementById('buildingUsage').selectedIndex = 0;
      document.getElementById('financingType').selectedIndex = 0;
      document.getElementById('financingPercent').value = "30";
      document.getElementById('annualRevenue').value = "";
      document.getElementById('annualExpenses').value = "";
      document.getElementById('interestRate').value = "8";
      document.getElementById('projectDuration').value = "10";
      if(document.getElementById('useInflation')) {
        document.getElementById('useInflation').checked = false;
        document.getElementById('inflationRate').value = "3";
      }
      document.getElementById('floorPlanPreview').innerHTML = "";
      document.getElementById('resultSection').innerHTML = "<h2>النتائج والتقارير</h2><p id='costAnalysis'></p><canvas id='costChart' style='max-width: 800px; margin: 20px auto;'></canvas>";
      document.getElementById('scenarioResults').innerHTML = "";
      document.getElementById('scenarioComparison').innerHTML = "";
      localStorage.removeItem('designInputs');
    }
    
    // دوال التصدير والطباعة
    function exportPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF("p", "mm", "a4");
      doc.setFont("Arial", "normal");
      doc.text("تقرير أداة التصميم والتخطيط الذكي للمنشآت", 20, 20);
      const results = document.getElementById("resultSection").innerText;
      doc.text(results, 20, 40);
      doc.text("يرجى مراجعة المخطط والمعطيات للحصول على التفاصيل الكاملة.", 20, 60);
      doc.save("تقرير_التصميم.pdf");
    }
    
    function exportExcel() {
      let data = [
        ["المخطط الأولي", document.getElementById('floorPlanPreview').innerText],
        ["نتائج التحليل", document.getElementById('resultSection').innerText]
      ];
      let ws = XLSX.utils.aoa_to_sheet(data);
      let wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, "التصميم");
      XLSX.writeFile(wb, "تقرير_التصميم.xlsx");
    }
    
    function printResult() {
      window.print();
    }
    
    // دالة تحليل السيناريوهات (مثال مبسط)
    function calculateScenario(scenarioName) {
      generateFloorPlan();
      const details = "تم استخدام المعطيات الحالية لتوليد المخطط الأولي.";
      updateScenarioResult(details, "", "", "", scenarioName);
    }
    
    function updateScenarioResult(financing, npv, payback, irr, scenarioName) {
      const scenarioDiv = document.getElementById('scenarioResults');
      scenarioDiv.innerHTML = `
        <h3>${scenarioName}</h3>
        <p><strong>${financing}</strong></p>
        <p><strong>${npv}</strong></p>
        <p><strong>${payback}</strong></p>
        <p><strong>${irr}</strong></p>
      `;
    }
    
    // حفظ السيناريو الحالي للمقارنة
    let savedScenarios = [];
    function saveScenario() {
      const scenario = {
        projectName: document.getElementById('projectName').value || "المشروع غير محدد",
        floorPlan: document.getElementById('floorPlanPreview').innerHTML,
        timestamp: new Date().toLocaleString()
      };
      savedScenarios.push(scenario);
      updateScenarioComparison();
    }
    
    function updateScenarioComparison() {
      const compDiv = document.getElementById('scenarioComparison');
      if (savedScenarios.length === 0) {
        compDiv.innerHTML = "<p>لا توجد سيناريوهات محفوظة.</p>";
        return;
      }
      let tableHTML = `<table>
                          <thead>
                            <tr style="background-color: var(--primary-color); color: #fff;">
                              <th style="padding: 10px;">التاريخ</th>
                              <th style="padding: 10px;">اسم المشروع</th>
                              <th style="padding: 10px;">عرض المخطط</th>
                            </tr>
                          </thead>
                          <tbody>`;
      savedScenarios.forEach(scn => {
        tableHTML += `<tr>
                        <td style="padding: 10px;">${scn.timestamp}</td>
                        <td style="padding: 10px;">${scn.projectName}</td>
                        <td style="padding: 10px;">
                          <button onclick="viewScenario('${scn.timestamp}')" class="btn-animate">
                            <i class="fas fa-eye"></i> عرض
                          </button>
                        </td>
                      </tr>`;
      });
      tableHTML += `</tbody></table>`;
      compDiv.innerHTML = `<h3>مقارنة السيناريوهات المحفوظة</h3>` + tableHTML;
    }
    
    function viewScenario(timestamp) {
      const scenario = savedScenarios.find(s => s.timestamp === timestamp);
      if (scenario) {
        document.getElementById('floorPlanPreview').innerHTML = scenario.floorPlan;
        alert("تم عرض السيناريو المحفوظ بتاريخ " + scenario.timestamp);
      }
    }
    
    /* --- قسم عرض التصميم ثنائي وثلاثي الأبعاد --- */
    function show2DView() {
      document.getElementById('view2DContainer').style.display = "block";
      document.getElementById('view3DContainer').style.display = "none";
      render2DPlan();
    }
    function show3DView() {
      document.getElementById('view2DContainer').style.display = "none";
      document.getElementById('view3DContainer').style.display = "block";
      render3DPlan();
    }
    function render2DPlan() {
      const canvas = document.getElementById("view2D");
      const ctx = canvas.getContext("2d");
      canvas.width = canvas.offsetWidth;
      canvas.height = canvas.offsetHeight;
      ctx.fillStyle = "#fff";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.strokeStyle = "#8B4513";
      ctx.lineWidth = 2;
      let planWidth = canvas.width * 0.6;
      let planHeight = canvas.height * 0.6;
      let startX = (canvas.width - planWidth) / 2;
      let startY = (canvas.height - planHeight) / 2;
      ctx.strokeRect(startX, startY, planWidth, planHeight);
      ctx.fillStyle = "#333";
      ctx.font = "18px Cairo, sans-serif";
      ctx.fillText("اتجاه: " + getOrientationLabel(document.getElementById('buildingOrientation').value), startX, startY - 10);
      
      // تقسيم المخطط إلى أقسام مبسطة
      let bedroomsHeight = planHeight * 0.4;
      let bathroomsHeight = planHeight * 0.15;
      let kitchensHeight = planHeight * 0.15;
      let livingHeight = planHeight * 0.3;
      ctx.strokeStyle = "#FF5733";
      ctx.strokeRect(startX, startY, planWidth, bedroomsHeight);
      ctx.fillText("غرف النوم (" + document.getElementById('numRooms').value + ")", startX + 5, startY + 20);
      let bathroomsY = startY + bedroomsHeight;
      ctx.strokeStyle = "#33A1FF";
      ctx.strokeRect(startX, bathroomsY, planWidth, bathroomsHeight);
      ctx.fillText("الحمامات (" + document.getElementById('numBathrooms').value + ")", startX + 5, bathroomsY + 20);
      let kitchensY = bathroomsY + bathroomsHeight;
      ctx.strokeStyle = "#33FF77";
      ctx.strokeRect(startX, kitchensY, planWidth, kitchensHeight);
      ctx.fillText("المطابخ (" + document.getElementById('numKitchens').value + ")", startX + 5, kitchensY + 20);
      let livingY = kitchensY + kitchensHeight;
      ctx.strokeStyle = "#FF33A8";
      ctx.strokeRect(startX, livingY, planWidth, livingHeight);
      ctx.fillText("غرف المعيشة (" + document.getElementById('numLivingRooms').value + ")", startX + 5, livingY + 20);
    }
    
    let threeScene, threeCamera, threeRenderer, orbitControls;
    function render3DPlan() {
      const container = document.getElementById("view3DContainer");
      while (container.firstChild) { container.removeChild(container.firstChild); }
      threeScene = new THREE.Scene();
      threeCamera = new THREE.PerspectiveCamera(75, container.offsetWidth / container.offsetHeight, 0.1, 1000);
      threeRenderer = new THREE.WebGLRenderer({ antialias: true });
      threeRenderer.setSize(container.offsetWidth, container.offsetHeight);
      container.appendChild(threeRenderer.domElement);
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
      threeScene.add(ambientLight);
      const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
      directionalLight.position.set(10, 10, 10);
      threeScene.add(directionalLight);
      const gridHelper = new THREE.GridHelper(20, 20);
      threeScene.add(gridHelper);
      const geometry = new THREE.BoxGeometry(4, 2, 6);
      const material = new THREE.MeshLambertMaterial({ color: 0x8B4513 });
      const building = new THREE.Mesh(geometry, material);
      building.position.set(0, 1, 0);
      threeScene.add(building);
      threeCamera.position.set(10, 10, 10);
      orbitControls = new THREE.OrbitControls(threeCamera, threeRenderer.domElement);
      orbitControls.enableDamping = true;
      orbitControls.dampingFactor = 0.1;
      function animate() {
        requestAnimationFrame(animate);
        orbitControls.update();
        threeRenderer.render(threeScene, threeCamera);
      }
      animate();
    }
    
    /* --- قسم عرض النتائج والتقارير النهائية مع الرسم البياني (Chart.js) --- */
    function updateCostChart() {
      const ctx = document.getElementById('costChart').getContext('2d');
      const costAnalysis = calculateCostAnalysis();
      // بيانات افتراضية لتحليل التكاليف (يمكن تطويرها بناءً على معطيات أكثر تفصيلاً)
      const data = {
        labels: ['التكلفة لكل م²'],
        datasets: [{
          label: 'التكلفة (بالعملة المختارة)',
          data: [parseFloat(costAnalysis.split(': ')[1]) || 0],
          backgroundColor: ['rgba(139, 69, 19, 0.6)'],
          borderColor: ['rgba(139, 69, 19, 1)'],
          borderWidth: 1
        }]
      };
      if(window.costChartInstance) {
        window.costChartInstance.destroy();
      }
      window.costChartInstance = new Chart(ctx, {
        type: 'bar',
        data: data,
        options: {
          responsive: true,
          scales: {
            y: { beginAtZero: true }
          }
        }
      });
    }
  </script>
