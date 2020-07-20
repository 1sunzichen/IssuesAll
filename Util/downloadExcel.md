```
  export const downloadCsv = (linkId, header, content, fileName) => {
	if (!linkId || !header || !content) {
		// console.log("downloadCsv 参数 error");
		return false;
	}

	const downloadLink = document.getElementById(linkId);
	let context = `${header.join(',') }\n`;
	for (let i = 0; i < content.length; i++) {
		const item = content[i];
		item.forEach((item, index, list) => {
			context = `${context + item  },`;
		});
		context += '\n';
	}

	// console.log('---------------拼接的字符串---------------\n' + context)
	// let context = "col1," + "反反\r复复" + ",col3\nvalue1,value2,value3"
	context = encodeURIComponent(context);
	downloadLink.download = `${fileName  }.csv`; // 下载的文件名称
	downloadLink.href = `data:text/csv;charset=utf-8,\ufeff${  context}`; // 加上 \ufeff BOM 头
	downloadLink.addEventListener(
		'click',
		function(e) {
			e.stopPropagation();
		},
		false,
	); // 阻止冒泡事件
	downloadLink.click();
 };
```
linkId, a标签id
header,  表头
content, 内容  
fileName 文件名字
```
const exportParams = () => {
		getMemberInfoList({...searchValue,current:1,pageSize:"999999"}).then(res=>{
			// console.log(res,"resxxx");
			const currentTime = moment(new Date()).format('YYYY-MM-DD');
			const fileName = `会员信息${  currentTime}`;
			const linkId = "download-linkN";
			
			const header = ["会员ID","手机号","归属商户","区域","储值卡余额","服务使用总次数","注册时间	","最近登陆时间"]
			
			const params = ['memberId',"phoneNumber","ownedMerchant","provinceName","balanceOfSavingsCard","totalTimes","registrationTime","lastLoginTime"]
			const content = [];
			res.data.forEach(item => {
				const rowContent = [];
				params.forEach(data=>{		
					if(data==='provinceName'){
						rowContent.push(item["provinceName"]+item["cityName"]+item["districtName"])
					}
					rowContent.push(item[data])
				})
				content.push(rowContent);
			});
	
			downloadCsv(linkId, header, content, fileName);
		},()=>{ message.error('导出失败'); })
	};
```
