JavaScript DOM 读书笔记 9
========================

## HTML5

1. HTML5检测

```
Modernizr:
http://www.modernizr.com/
```

2. 灰度图片

```
基于用户操作交互时才能体现canvas优势,但也不能滥用。
```

```html
<!-- grayscale.html -->
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8" />
	<title>Grayscale Canvas Example</title>
	<script src="scripts/modernizr-1.6.min.js"></script>
</head>
<body>
    <img src="images/avatar.png" id="avatar" title="Jeffrey Sambells" alt="My Avatar"/>
    <script src="scripts/grayscale.js"></script>
</body>
</html>
```

```javascript
// grayscale.js
function convertToGS(img) {
  // For good measure return if canvas isn't supported.
  if (!Modernizr.canvas) return;
  
  // Store the original color version.
  img.color = img.src;
  
  // Create a grayscale version.
  img.grayscale = createGSCanvas(img);
  
  // Swap the images on mouseover/out 
	img.onmouseover = function() {
	  this.src = this.color;
	} 
	img.onmouseout = function() {
 		this.src = this.grayscale; 
 	}
 	
 	img.onmouseout();
} 

function createGSCanvas(img) {

	var canvas=document.createElement("canvas");
	canvas.width=img.width;
	canvas.height=img.height;
	var ctx=canvas.getContext("2d");
	ctx.drawImage(img,0,0);
	
	// Note: getImageData will only work for images 
	// on the same domain as the script. 
	var c = ctx.getImageData(0, 0, img.width, img.height); 
	for (i=0; i<c.height; i++) {  // 为什么这样会变成灰色图片?
		for (j=0; j<c.width; j++) {
			var x = (i*4) * c.width + (j*4);  // 这个x为什要这样取值
			var r = c.data[x]; 
			var g = c.data[x+1]; 
			var b = c.data[x+2]; 
			c.data[x] = c.data[x+1] = c.data[x+2] = (r+g+b)/3;
		}
	}
	ctx.putImageData(c,0,0,0,0, c.width, c.height);
	return canvas.toDataURL();
}

// Add a load event. 
// use addLoadEvent function if alongside other scripts. 
window.onload = function() {
	convertToGS(document.getElementById('avatar'));
}
```

3. 视频

```html
<!-- player.html -->
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
    <title>My Video</title>
    <link rel="stylesheet" href="styles/player.css" />
</head>
<body>
<div class="video-wrapper">
    <video id="movie" controls>
        <source src="http://media.w3.org/2010/05/sintel/trailer.mp4" />
        <source src="http://media.w3.org/2010/05/sintel/trailer.webm" 
        type='video/webm; codecs="vp8, vorbis"' />
        <source src="http://media.w3.org/2010/05/sintel/trailer.ogv" 
        type='video/ogg; codecs="theora, vorbis"' />
        <p>Download movie as 
        <a href="http://media.w3.org/2010/05/sintel/trailer.mp4">MP4</a>, 
        <a href="http://media.w3.org/2010/05/sintel/trailer.webm">WebM</a>, 
        or <a href="http://media.w3.org/2010/05/sintel/trailer.ogv">Ogg</a>.</p> 
    </video>
</div>
    <script src="scripts/player.js"></script>
</body>
</html>
```

```javascript
// player.js
(function() {

function createVideoControls() {
    var vids = document.getElementsByTagName('video');
    for (var i = 0 ; i < vids.length ; i++) {
        addControls( vids[i] );
    }
}

function addControls( vid ) {

    vid.removeAttribute('controls');


    vid.height = vid.videoHeight;
    vid.width = vid.videoWidth;
    vid.parentNode.style.height = vid.videoHeight + 'px';
    vid.parentNode.style.width = vid.videoWidth + 'px';

    var controls = document.createElement('div');
    controls.setAttribute('class','controls');
        
    var play = document.createElement('button');
    play.setAttribute('title','Play');
    play.innerHTML = '&#x25BA;';
    
    controls.appendChild(play);
    
    vid.parentNode.insertBefore(controls, vid);
    
    play.onclick = function () {
        if (vid.ended) {
            vid.currentTime = 0;
        }
        if (vid.paused) {
            vid.play();
        } else {
            vid.pause();
        }
    };  
    
    vid.addEventListener('play', function () {
        play.innerHTML = '&#x2590;&#x2590;';
        play.setAttribute('paused', true);
    }, false);
    
    vid.addEventListener('pause', function () {
        play.removeAttribute('paused');
        play.innerHTML = '&#x25BA;';
    }, false);
    
    vid.addEventListener('ended', function () {
        vid.pause();  // 视频播放完自动暂停,显示的图标是播放
    }, false);
}

window.onload = function() {
    createVideoControls();
}

})()
```

4. 表单

```javascript
// 检查是否支持某种类型的输入控件
function inputSupportsType(type) {
    if (!document.createElement) return false;
    var input = document.createElemen('input');
    input.setAttribute('type', type);
    if (input.type == 'text' && type != 'text') {
        return false;
    } else {
        return true;
    }
}
```

```html
<input type="date or other types" name="input-name">
```