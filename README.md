<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>حمادة TV</title>
   <link rel="icon" href="https://raw.githubusercontent.com/1ffu1/hamada-tv/refs/heads/main/logo.png">
  <script src="https://content.jwplatform.com/libraries/Z79JsmAO.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/showdown/1.7.1/showdown.min.js"></script>
  <script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
  <script src="https://developer-tools.jwplayer.com/js/tools.min.js"></script>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <style>
    html, body { margin: 0; padding: 0; height: 100%; overflow: hidden; }
    .jwplayer { position: relative; width: 100%; height: 100%; }
    .jwplayer .jw-media video { width: 100%; height: 100%; object-fit: fill; }

    /* إضافة تأثير ضبابي لخلفية مشغل JWPlayer */
    .jw-controlbar.jw-reset , .jw-reset.jw-settings-menu , .resizeButton
    {
      background: #00000043; /* لون الخلفية المطلوب */
      backdrop-filter: blur(10px); /* إضافة تأثير الضبابية */
      -webkit-backdrop-filter: blur(10px); /* دعم للمستعرضات القديمة */
    }

    #resizeButton { position: absolute; top: 20px; left: 20px; background-color: rgba(0,0,0,0.4); color: white; border: none; padding: 10px 15px; font-size: 16px; cursor: pointer; border-radius: 5px; z-index: 9999; }
    #resizeButton:hover { background-color: rgba(0,0,0,0.6); }
  </style>
</head>
<body>
  <button id="resizeButton" class="resizeButton">
    <i class="fas fa-compress"></i>
  </button>
  <div id="my-jwplayer"></div>
   
  <script>
    // استخراج الرابط من معلمات URL
    const urlParams = new URLSearchParams(window.location.search);
    const videoUrl = "https://live-d-02-todtv-db.akamaized.net/variant/v1blackout/spo-hd-39-d-shortdvr/DASH_DASH/Live/channel(spo-hd-09)/manifest.mpd?hdnts=st=1741797944~exp=1741812344~acl=/variant/v1blackout/spo-hd-39-d-shortdvr/*~data=f5ccf9b9-c21d-4e49-8e38-96ee076ed988~hmac=06e37ed90544f4ad27c7a6f546f3ede4dc4eeb65c67a87b04246b0154440f994"; // الحصول على قيمة videoUrl من URL

    // إذا لم يتم تقديم رابط، يتم استخدام الرابط الذي قدمته
    const defaultVideoUrl = "https://hamada-tv.com/none.mp4";

    const finalVideoUrl = videoUrl || defaultVideoUrl; // استخدام الرابط من URL أو الرابط الافتراضي

    // إعداد اللاعب باستخدام الرابط النهائي
    var player = jwplayer("my-jwplayer").setup({
      file: finalVideoUrl, // استخدام الرابط النهائي
      width: "100%",
      height: "100%",
      repeat: true,
      volume: 100,
      autostart: true,
      mute: false,
      
      drm: {
        clearkey: {
          keyId: "0a7934dddc3136a6922584b96c3fd1e5",
          key: "676e6d1dd00bfbe266003efaf0e3aa02"
        }
      },
      skin: {
        name: "seven",
        active: "#FFA000",
        inactive: "#ffffffc9",
        background: "#00000020"
      }
    });

    // في حال حدوث خطأ أثناء التشغيل باستخدام clearkey، يتم إعادة تهيئة اللاعب باستخدام clearKeys
    player.on('error', function(error) {
      console.error("الطريقة الأولى فشلت (" + error.message + ")، سيتم استخدام طريقة clearKeys");
      player.setTextTrackVisibility(true);
      jwplayer("my-jwplayer").remove();
      jwplayer("my-jwplayer").setup({
        file: finalVideoUrl, // استخدام الرابط النهائي
        repeat: true,
        width: "100%",
        height: "100%",
        volume: 100,
        autostart: true,
        mute: false,
        drm: {
          clearKeys: {
            "0a7934dddc3136a6922584b96c3fd1e5": "676e6d1dd00bfbe266003efaf0e3aa02"
          }
        },
        skin: {
          name: "seven",
          active: "#FFA000",
          inactive: "#7f8c8d",
          background: "#00000020"
        }
      });
    });

    // ضبط خاصية object-fit للفيديو عند تحميل الصفحة
    window.onload = function() {
      const videoElement = document.querySelector('.jwplayer .jw-media video');
      if (videoElement) {
        videoElement.style.objectFit = 'fill';
      }
    };

    // وظيفة تغيير حجم الفيديو عند الضغط على زر التكبير/التصغير
    let isZoomed = false;
    document.getElementById('resizeButton').addEventListener('click', function() {
      const videoElement = document.querySelector('.jwplayer .jw-media video');
      if (videoElement) {
        if (!isZoomed) {
          videoElement.style.objectFit = 'contain';
          document.querySelector('#resizeButton i').className = 'fas fa-expand';
        } else {
          videoElement.style.objectFit = 'fill';
          document.querySelector('#resizeButton i').className = 'fas fa-compress';
        }
        isZoomed = !isZoomed;
      }
    });
  </script>
</body>
</html> 
