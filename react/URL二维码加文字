```
const downloadIamge=useCallback((index, currentName)=>{
			const Qr=document.getElementsByClassName('qrid')[index];  
			
			const image = new Image();
			image.src = Qr.toDataURL("image/png");
			// const a_link=document.getElementsByClassName('aId')[index];
			// a_link.href=image.src;
			getImg(currentName,16,image.src);
			// a_link.download=id;//好像这一行可以不写，，你可以尝试一下
			setTimeout(()=>{
				exportCanvasAsPNG("myCanvas", currentName)
			},500)
		},[ ] )
```
```

function getImg(text,fsz,imgUrl){ // 画布
			const c = document.getElementById("myCanvas");
			const cxt = c.getContext("2d");
			cxt.clearRect(0,0,300,300);
			const img = new Image();
			img.src = imgUrl;
			img.onload=function(){// 图片加载完成，才可处理
				//  cxt.drawImage(img,0,0,280,280,0,0,280,280);
				cxt.drawImage(img,0,0,100,100,40,0,280,280);
				cxt.save();
				cxt.font = `${fsz}px Microsoft YaHei`;
				cxt.textBaseline = 'middle';// 更改字号后，必须重置对齐方式，否则居中麻烦。设置文本的垂直对齐方式
				cxt.textAlign = 'center';
				const tw = cxt.measureText(text).width;
				const ftop = c.height/1.1;
				const fleft = c.width/2;
				cxt.fillStyle="#fff";  // 文字背景色
				cxt.fillRect(fleft-tw/2,ftop-fsz/2,tw,fsz);// 矩形在画布居中方式
				cxt.fillStyle="#ffffff";
				cxt.fillText(text,fleft,ftop);// 文本元素在画布居中方式
				cxt.strokeStyle = '#000';
				cxt.strokeText(text,fleft,ftop);// 文字边框
			}; 
		}
```
```
	//  导出图片方法
		function exportCanvasAsPNG(id, fileName) {
			const canvasElement = document.getElementById(id);
			const MIME_TYPE = "image/png";
			const imgURL = canvasElement.toDataURL(MIME_TYPE);
			const dlLink = document.createElement('a');
			dlLink.download = fileName;
			dlLink.href = imgURL;
			dlLink.dataset.downloadurl = [MIME_TYPE, dlLink.download, dlLink.href].join(':');
			document.body.appendChild(dlLink);
			dlLink.click();
			document.body.removeChild(dlLink);
		}
```
