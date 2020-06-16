1.Upload 阿里云OSS上传多张图片
环境 antdpro reacthooks
1.1-1
```
  import OSS from 'ali-oss';
```
1.1-2
```
    // 业务场景: 通过后台接口 获取 ossTokencurrent 对象
    // 核心代码  
		const client = new OSS({
			region: ossTokencurrent.region,
			accessKeyId: ossTokencurrent.accesKeyId,//
			accessKeySecret: ossTokencurrent.accesKeySecret,//
			stsToken: ossTokencurrent.securityToken, //
			bucket: ossTokencurrent.bucket, //
		})
     // put方法上传 
		const rl = await client.put(`/ptd/washService${Date.now()}`, file)
		if (rl) {
			return rl.url
		}
```
1.1-3 完整方法
```
const getUrl = useCallback(async file => {
	if (ossTokencurrent && JSON.stringify(ossTokencurrent) !== "{}") {
		if (ossTokencurrent.expiration > Date.now()) { // 没有过期
			const client = new OSS({
				region: ossTokencurrent.region,
				accessKeyId: ossTokencurrent.accesKeyId,//
				accessKeySecret: ossTokencurrent.accesKeySecret,//
				stsToken: ossTokencurrent.securityToken, //
				bucket: ossTokencurrent.bucket, //
		})
	const rl = await client.put(`/ptd/washService${Date.now()}`, file)
		if (rl) {
			return rl.url
		}

	}
	else {
	getStsToken().then(res => {
		console.log("expiration过期了", res)
		if (res.msg === "ok") {
			getUrl(file)
		}
	})
}
	}
}, [ossTokencurrent, currentimgUrl])
```
1.1-4 beforeUpload
```
  // 上传之前
	const beforeUpload = useCallback(async file => {  // 上传文件之前的钩子
	const res = await getUrl(file)
	const lastcurrentimgUrl =await  currentimgUrl.length > 0 
	? currentimgUrl[currentimgUrl.length - 1] : {uid: '1'};
	const resData = await{ uid: lastcurrentimgUrl.uid + 1, name: file.name, url: res };
	const newcurrentimgUrl = [resData, ...currentimgUrl]
		// return resData
	setImgUrl(newcurrentimgUrl);
	}, [ossTokencurrent, currentimgUrl])
```
1.1-5 Upload组件
```
  <Upload
	beforeUpload={beforeUpload}
	customRequest={()=>false}
	name="file"
	listType="picture-card"
	showUploadList={{
		showPreviewIcon:false,
	}}
        fileList={currentimgUrl}
	onChange={({file,fileList}) => {
		if(file.status==='removed'){
		setImgUrl(fileList)
		}
	}}
	>
	{currentimgUrl.length >= 8 ? null : <PlusOutlined />}
	</Upload>
```
