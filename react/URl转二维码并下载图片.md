环境： antd table + react 
1.需要一个npm包   二维码的包
```
  npm install qrcode.react
```
2.然后 
```
    const url = `xxx`
		return <div style={{display:"flex",justfyContent:"space-between"}}>
				
		<a 
			className="aId"
			title="点我下载"
			key="下载二维码" 
			download
			onClick={()=>{downloadIamge(index)}}		
			>
			下载二维码
			<QRCode
					style={{display:"none"}}
					className="qrid"
					value={url}  // value参数为生成二维码的链接
					size={100} // 二维码的宽高尺寸
					fgColor="#000000"  // 二维码的颜色
								/>
					</a>
		</div>
```
3.方法
```
const downloadIamge=index=>{
			const Qr=document.getElementsByClassName('qrid')[index];  
			
			const image = new Image();
			image.src = Qr.toDataURL("image/png");
			const a_link=document.getElementsByClassName('aId')[index];
			a_link.href=image.src;
			// console.log("imgae.src++===",image.src)
			// a_link.download=id;//好像这一行可以不写，，你可以尝试一下
		}
```
