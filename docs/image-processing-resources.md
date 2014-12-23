### [Project Naptha](http://projectnaptha.com/)
Project Naptha automatically applies state-of-the-art computer vision algorithms on every image you see while browsing the web. The result is a seamless and intuitive experience, where you can highlight as well as copy and paste and even edit and translate the text formerly trapped within an image.

### References

[Utilize File API and Canvas to dynamically generate thumbnails of images](http://stackoverflow.com/questions/4773966/drawing-an-image-from-a-data-url-to-a-canvas)

```javascript
(function (doc) {
    var oError = null;
    var oFileIn = doc.getElementById('fileIn');
    var oFileReader = new FileReader();
    var oImage = new Image();
    oFileIn.addEventListener('change', function () {
        var oFile = this.files[0];
        var oLogInfo = doc.getElementById('logInfo');
        var rFltr = /^(?:image\/bmp|image\/cis\-cod|image\/gif|image\/ief|image\/jpeg|image\/jpeg|image\/jpeg|image\/pipeg|image\/png|image\/svg\+xml|image\/tiff|image\/x\-cmu\-raster|image\/x\-cmx|image\/x\-icon|image\/x\-portable\-anymap|image\/x\-portable\-bitmap|image\/x\-portable\-graymap|image\/x\-portable\-pixmap|image\/x\-rgb|image\/x\-xbitmap|image\/x\-xpixmap|image\/x\-xwindowdump)$/i
        try {
            if (rFltr.test(oFile.type)) {
                oFileReader.readAsDataURL(oFile);
                oLogInfo.setAttribute('class', 'message info');
                throw 'Preview for ' + oFile.name;
            } else {
                oLogInfo.setAttribute('class', 'message error');
                throw oFile.name + ' is not a valid image';
            }
        } catch (err) {
            if (oError) {
                oLogInfo.removeChild(oError);
                oError = null;
                $('#logInfo').fadeOut();
                $('#imgThumb').fadeOut();
            }
            oError = doc.createTextNode(err);
            oLogInfo.appendChild(oError);
            $('#logInfo').fadeIn();
        }
    }, false);
    oFileReader.addEventListener('load', function (e) {
        oImage.src = e.target.result;
    }, false);
    oImage.addEventListener('load', function () {
        if (oCanvas) {
            oCanvas = null;
            oContext = null;
            $('#imgThumb').fadeOut();
        }
        var oCanvas = doc.getElementById('imgThumb');
        var oContext = oCanvas.getContext('2d');
        var nWidth = (this.width > 500) ? this.width / 4 : this.width;
        var nHeight = (this.height > 500) ? this.height / 4 : this.height;
        oCanvas.setAttribute('width', nWidth);
        oCanvas.setAttribute('height', nHeight);
        oContext.drawImage(this, 0, 0, nWidth, nHeight);
        $('#imgThumb').fadeIn();
    }, false);
})(document);
```

[Resizing images with Javascript](http://blog.liip.ch/archive/2013/05/28/resizing-images-with-javascript.html). Its sample codes are placed at [Github: js-resize](https://github.com/nicam/js-resize)

[Using CORS](http://www.html5rocks.com/en/tutorials/cors/), Cross-Origin Resource Sharing.

[HTTP access control (CORS)](https://developer.mozilla.org/zh-TW/docs/HTTP/Access_control_CORS)

[CORS enabled image](https://developer.mozilla.org/en-US/docs/HTML/CORS_Enabled_Image)

```conf
<IfModule mod_setenvif.c>
    <IfModule mod_headers.c>
        <FilesMatch "\.(cur|gif|ico|jpe?g|png|svgz?|webp)$">
            SetEnvIf Origin ":" IS_CORS
            Header set Access-Control-Allow-Origin "*" env=IS_CORS
        </FilesMatch>
    </IfModule>
</IfModule>
```

[Sending and Receiving Binary Data](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Sending_and_Receiving_Binary_Data)

```javascript
var oReq = new XMLHttpRequest();
oReq.open("GET", "/myfile.png", true);
oReq.responseType = "arraybuffer";

oReq.onload = function(oEvent) {
  var blob = new Blob([oReq.response], {type: "image/png"});
  // ...
};

oReq.send();
```

[createObjectURL](http://stackoverflow.com/questions/18976327/binary-array-to-canvas)

```javascript
var imageData = GetTheTypedArraySomehow();
var blob = new Blob([imageData], {type: "image/jpeg"});
var url = URL.createObjectURL(blob);

...

var img = new Image();
img.onload = function() {
    /// draw image to canvas
    context.drawImage(this, x, y);
}
img.src = url;
```


### UVC Errors

[libv4l2 error turning on stream: No space left on device](http://blog.roodo.com/rocksaying/archives/25359530.html), 當開啟兩隻以上的 USB Video Camera (WebCam) 時，有時會發生 "No space left on device" 的錯誤。「無多餘空間」並不是指磁碟與記憶體沒有空間，而是 USB 的資料頻寬。 關於 WebCam 影像傳輸所需的資料頻寬可參考下列文章。

> Apparently it's caused by webcams requesting all the available bandwidth on the USB host controller. With that in mind I decided to run wireshark and capinfos to see just how much bandwidth a single camera used.
> 
> 4 megabits per second at 320x240
>
> 14 megabits per second at 640x480
>
> 32 megabits per second at 1280x720
>
> Interesting! That might explain why two cameras at 320x240 work but any higher resolution fails.

(yagamy: USB2 speed is 480Mbits, USB3 speed is 5Gbits, but USB1.1 is only 12Mbits)
