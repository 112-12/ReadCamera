<!DOCTYPE html>
<html>
<head>
  <base target="_top">
  <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
  <h2>設備攝影表單</h2>

  <!-- 表單 -->
  <form id="myForm">
    姓名：<br>
    <input type="text" id="name" required><br><br>

    攝影畫面：<br>
    <video id="video" autoplay playsinline width="300"></video><br><br>

    <button type="button" onclick="takePhoto()">拍照</button><br><br>

    <canvas id="canvas" width="300" height="200" style="display:none;"></canvas>
    <img id="photo" width="300"><br><br>

    <button type="submit">送出</button>
  </form>

  <script>
    let stream;

    // 啟動攝影機（支援手機/平板/桌機）
    async function startCamera() {
      try {
        stream = await navigator.mediaDevices.getUserMedia({
          video: { facingMode: "environment" }, // 手機優先後鏡頭
          audio: false
        });
        document.getElementById('video').srcObject = stream;
      } catch (e) {
        alert("無法開啟攝影機：" + e);
      }
    }

    // 拍照
    function takePhoto() {
      const video = document.getElementById('video');
      const canvas = document.getElementById('canvas');
      const ctx = canvas.getContext('2d');

      ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

      const dataUrl = canvas.toDataURL('image/png');
      document.getElementById('photo').src = dataUrl;
    }

    // 表單送出
    document.getElementById('myForm').addEventListener('submit', function(e) {
      e.preventDefault();

      const name = document.getElementById('name').value;
      const image = document.getElementById('photo').src;

      if (!image) {
        alert("請先拍照");
        return;
      }

      console.log("姓名:", name);
      console.log("圖片(base64):", image);

      alert("已送出");
    });

    // 初始化
    window.onload = startCamera;
  </script>
</body>
</html>
