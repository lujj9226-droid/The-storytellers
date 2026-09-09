# The-storytellers

<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>The Storytellers Jeddah | شاركونا قصصكم</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }
    body { background-color: #0b0d12; color: #f8fafc; display: flex; flex-direction: column; justify-content: center; align-items: center; min-height: 100vh; padding: 20px; }
    
    .card { background: #161a23; border: 1px solid #2a3142; padding: 30px; border-radius: 24px; width: 100%; max-width: 480px; box-shadow: 0 20px 40px rgba(225, 29, 72, 0.15); text-align: center; position: relative; overflow: hidden; }
    .card::before { content: ""; position: absolute; top: 0; left: 0; right: 0; height: 4px; background: linear-gradient(90deg, #e11d48, #f43f5e); }
    
    .logo { font-size: 28px; font-weight: 800; letter-spacing: 1px; color: #e11d48; margin-bottom: 8px; text-transform: uppercase; }
    .sub-title { color: #94a3b8; font-size: 14px; margin-bottom: 25px; }
    
    .input-group { margin-bottom: 18px; text-align: right; }
    label { font-size: 13px; font-weight: 600; color: #cbd5e1; display: block; margin-bottom: 6px; }
    input[type="text"], input[type="file"] { width: 100%; padding: 14px; border-radius: 12px; border: 1px solid #2a3142; background: #0f1219; color: white; font-size: 14px; outline: none; transition: 0.2s; }
    input[type="text"]:focus { border-color: #e11d48; }
    input[type="file"] { cursor: pointer; color: #94a3b8; }
    
    .upload-btn { width: 100%; padding: 16px; background: linear-gradient(135deg, #e11d48, #be123c); border: none; border-radius: 12px; color: white; font-weight: 700; font-size: 16px; cursor: pointer; margin-top: 10px; transition: 0.3s; box-shadow: 0 4px 15px rgba(225, 29, 72, 0.3); }
    .upload-btn:hover { opacity: 0.9; transform: translateY(-1px); }
    
    #status { margin-top: 15px; font-size: 14px; font-weight: 600; color: #f43f5e; }
    
    /* شاشة الشكر */
    .thank-you-screen { display: none; text-align: center; animation: fadeIn 0.5s ease-in-out; }
    .thank-you-icon { font-size: 50px; margin-bottom: 15px; }
    .thank-you-title { font-size: 24px; color: #e11d48; font-weight: bold; margin-bottom: 10px; }
    .thank-you-text { color: #cbd5e1; font-size: 15px; line-height: 1.6; margin-bottom: 25px; }
    
    /* قسم الحسابات */
    .social-section { margin-top: 25px; border-top: 1px solid #2a3142; padding-top: 20px; text-align: center; }
    .social-section p { font-size: 13px; color: #94a3b8; margin-bottom: 14px; }
    .social-links { display: flex; justify-content: center; gap: 12px; flex-wrap: wrap; }
    .social-btn { display: flex; align-items: center; gap: 8px; padding: 12px 18px; background: #0f1219; border: 1px solid #2a3142; border-radius: 30px; color: #e2e8f0; text-decoration: none; font-size: 14px; font-weight: 600; transition: 0.3s; }
    .social-btn:hover { border-color: #e11d48; color: #e11d48; transform: translateY(-2px); }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>

  <div class="card">
    <!-- نموذج الرفع -->
    <div id="uploadFormContainer">
      <div class="logo">The Storytellers</div>
      <div class="sub-title">جدة 📍 | شاركونا لقطاتكم ولحظاتكم المميزة</div>

      <form id="storyForm">
        <div class="input-group">
          <label for="fileInput">اختر صورة أو فيديو 📸🎥</label>
          <input type="file" id="fileInput" accept="image/*,video/*" required>
        </div>

        <div class="input-group">
          <label for="instagram">حسابك في إنستغرام (Instagram) 📸</label>
          <input type="text" id="instagram" placeholder="@username" required>
        </div>

        <div class="input-group">
          <label for="tiktok">حسابك في تيك توك (TikTok) 🎵</label>
          <input type="text" id="tiktok" placeholder="@username">
        </div>

        <button type="submit" class="upload-btn" id="submitBtn">إرسال القصة ✨</button>
      </form>

      <div id="status"></div>
    </div>

    <!-- شاشة الشكر والحسابات (تظهر بعد الرفع) -->
    <div id="thankYouScreen" class="thank-you-screen">
      <div class="thank-you-icon">❤️✨</div>
      <div class="thank-you-title">شكرًا لمشاركتك معنا!</div>
      <div class="thank-you-text">
        تم استلام قصتك بنجاح 🎬<br>
        سعداء بوجودك وجزء من مجتمع <b>The Storytellers</b> في جدة.
      </div>

      <div class="social-section">
        <p>تابعوا تغطياتنا وحساباتنا الرسمية:</p>
        <div class="social-links">
          <a href="https://www.instagram.com/thestorytellers.sa/" target="_blank" class="social-btn">Instagram 📸</a>
          <a href="https://www.tiktok.com/@thestorytellers.sa" target="_blank" class="social-btn">TikTok 🎵</a>
        </div>
      </div>
    </div>
  </div>

  <script>
    // ضعي بيانات حسابك في Cloudinary هنا
    const CLOUD_NAME = 'YOUR_CLOUD_NAME'; 
    const UPLOAD_PRESET = 'YOUR_UPLOAD_PRESET';

    const form = document.getElementById('storyForm');
    const statusDiv = document.getElementById('status');
    const uploadFormContainer = document.getElementById('uploadFormContainer');
    const thankYouScreen = document.getElementById('thankYouScreen');

    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      statusDiv.style.color = '#e11d48';
      statusDiv.innerText = 'جاري رفع القصة... انتظر لحظات ⏳';

      const fileInput = document.getElementById('fileInput').files[0];
      const instagram = document.getElementById('instagram').value;
      const tiktok = document.getElementById('tiktok').value;

      const formData = new FormData();
      formData.append('file', fileInput);
      formData.append('upload_preset', UPLOAD_PRESET);
      formData.append('context', `instagram=${instagram}|tiktok=${tiktok}`);

      try {
        const response = await fetch(`https://api.cloudinary.com/v1_1/${CLOUD_NAME}/auto/upload`, {
          method: 'POST',
          body: formData
        });

        if (response.ok) {
          // إخفاء النموذج وإظهار شاشة الشكر
          uploadFormContainer.style.display = 'none';
          thankYouScreen.style.display = 'block';
        } else {
          throw new Error('فشل الرفع');
        }
      } catch (error) {
        statusDiv.style.color = '#f87171';
        statusDiv.innerText = 'حدث خطأ أثناء الرفع، جرب مرة أخرى.';
      }
    });
  </script>
</body>
</html>