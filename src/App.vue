<script setup>
import { onMounted } from 'vue';

onMounted(()=>{
  // 获取元素
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');
var uploadBtn = document.getElementById('uploadBtn');
// 监听文件上传事件
uploadBtn.addEventListener('change', function(e) {
    var file = e.target.files[0];
    
    // 创建图像对象
    var img = new Image();
    img.onload = function() {
        // 将图像绘制到画布上
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        // 应用卡通效果
        applyCartoonEffect();
    };
    img.src = URL.createObjectURL(file);
});
// 应用卡通效果
function applyCartoonEffect() {
    // 获取画布的像素数据
    var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
    var data = imageData.data;

    // 遍历每个像素点
    for (var i = 0; i < data.length; i += 4) {
        // 获取像素点的RGB值
        var r = data[i];
        var g = data[i + 1];
        var b = data[i + 2];

        // 计算灰度值
        var gray = (r + g + b) / 3;

        // 将像素点转换为卡通效果，即将灰度值处理为0或255
        if (gray < 128) {
            data[i] = data[i + 1] = data[i + 2] = 0; // 设置为黑色
        } else {
            data[i] = data[i + 1] = data[i + 2] = 255; // 设置为白色
        }
    }

    // 将处理后的像素数据重新绘制到画布上
    ctx.putImageData(imageData, 0, 0);

    var downloadLink = document.getElementById('downloadLink');
    downloadLink.href = canvas.toDataURL(); // 默认为PNG格式
}
})




</script>

<template>
  <main>
    <div class="wrapper">
      <input type="file" id="uploadBtn" accept="image/*">
      <a id="downloadLink" href="#" download="cartoon_avatar.png">下载</a>
    </div>
    <canvas id="canvas" width="300" height="300"></canvas>
  </main>
</template>

<style scoped>
canvas {
            border: 1px solid #000;
        }

.wrapper{
  width: 300px;
}
</style>
