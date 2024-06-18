<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>موقع نشر الفيديو</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    margin: 0;
    padding: 20px;
  }
  .container {
    max-width: 800px;
    margin: 0 auto;
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
  }
  .video-container {
    position: relative;
    width: 100%;
    padding-top: 56.25%; /* 16:9 Aspect Ratio */
    background-color: #000;
    border-radius: 8px;
    overflow: hidden;
    margin-bottom: 20px;
  }
  .video-container video {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
</style>
</head>
<body>

<div class="container">
  <h1>مرحباً بك في موقع نشر الفيديو</h1>
  
  <div class="video-container">
    <video controls>
      <source src="myvideo.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
  
  <form id="videoForm">
    <input type="text" id="videoTitle" placeholder="عنوان الفيديو">
    <input type="text" id="videoDescription" placeholder="hellozat">
    <input type="file" id="videoFile" accept="video/mp4">
    <button type="submit">نشر الفيديو</button>
  </form>
  
  <div id="message"></div>
</div>

<script>
document.getElementById('videoForm').addEventListener('submit', function(event) {
  event.preventDefault();
  
  var title = document.getElementById('videoTitle').value;
  var description = document.getElementById('videoDescription').value;
  var file = document.getElementById('videoFile').files[0];
  
  if (!title || !description || !file) {
    showMessage('الرجاء ملء جميع الحقول');
    return;
  }
  
  var formData = new FormData();
  formData.append('title', title);
  formData.append('description', description);
  formData.append('video', file);
  
  fetch('upload_video.php', {
    method: 'POST',
    body: formData
  })
  .then(response => response.json())
  .then(data => {
    if (data.success) {
      showMessage('تم نشر الفيديو بنجاح');
      document.getElementById('videoForm').reset();
    } else {
      showMessage('حدث خطأ أثناء نشر الفيديو');
    }
  })
  .catch(error => {
    console.error('Error:', error);
    showMessage('حدث خطأ أثناء الاتصال بالخادم');
  });
});

function showMessage(message) {
  document.getElementById('message').textContent = message;
}
</script>

</body>
</html>
