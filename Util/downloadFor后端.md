 
 技术栈 antd+ axios 二进制流下载
##  请求加入
```
 responseType: 'blob',
```
##  下载回显
```
api.downloadForStore(currentParams).then((resp)=>{
    console.log(resp,'resp');
    if(resp) {
      // const blodheader='\uFEFF';
      // let blob = new Blob([blodheader+resp]);
      let url = window.URL.createObjectURL(resp);
      let link = document.createElement('a');
      link.href = url
      link.download = '门店管理.xlsx'
      document.body.appendChild(link);
      link.click();//点击下载
      link.remove();//下载完成移除元素
      window.URL.revokeObjectURL(link.href); //用完之后使用URL.revokeObjectURL()释放；
    }else {
      Message.error('文件下载失败，请重试！');
    }
  })
```
## 相关代码 下载方法
```
  downloadForStore(params){
    return request({
      // headers: { 'Content-Type':"application/octet-stream"},
      responseType: 'blob',
      url: LOCAL_HOME + "/enterpriseStore/download",
      method: "POST",
      data:params,
      // loading: true
    });
  }
```
##  注意下载文件的返回结构
```
import Axios from "axios";
import get from "lodash/get";
import Loading from "@/components/Loading";
import qs from "qs";
import { message as Toast } from "antd";
import { getToken, gotoLogin } from "./userInfo";

const axios = Axios.create({
  // baseURL: API_URL, // url = base url + request url
  timeout: 10000, // request timeout
  headers: {
    "Content-Type": "application/json;charset=UTF-8"
  },
  withCredentials: true // 表示跨域请求时是否需要使用凭证，是否允许带cookie这些
});
axios.interceptors.request.use(
  function(config) {
    console.log(config,'config');
    // 在发送请求之前做些什么
    // config.headers["loginstring"] = getToken();
    if (config.formData) {
      config.headers["Content-Type"] = "application/x-www-form-urlencoded";
      if (config.method !== "get") {
        config.transformRequest = [
          data => {
            return qs.stringify(data);
          }
        ];
      }
    }
    if (config.loading) {
      Loading.show();
    }
    return config;
  },
  function(error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  }
);
axios.interceptors.response.use(
  res => {
    //关闭Loading
    Loading.hide();
    console.log(res,'res');
    if (res.status !== 200) {
      return Promise.reject(res);
    }
    const {
      data: { code, status, data, message, msg, out, rtn_code }
    } = res;
    if (
      res.status===200||
      status === "success" ||
      code === 200 ||
      code === 1 ||
      rtn_code === "0"
    ) {
      if(res.data instanceof Blob){
        return Promise.resolve(res.data);
      }
      else if(out) {
        return Promise.resolve(out);
      }
      else {
        return Promise.resolve(data);
      }
    }
    if (status === "nonLogin") {
      gotoLogin();
      return Promise.reject(res);
    }
    if (status === "failed" && get(res, "data.data.reason_type", "") !== "") {
      //不弹toast
      return Promise.reject(res);
    }
    //java接口登录状态失效
    if (
      (code && code == 401) ||
      (code && code == 403) ||
      (msg && msg.includes("非法用户")) ||
      (message && message.includes("非法用户")) ||
      (msg && msg.includes("凭证")) ||
      (message && message.includes("凭证")) ||
      (rtn_code && rtn_code == 401)
    ) {
      gotoLogin();
      return Promise.reject(res);
    }

    Toast.error(message || msg || "数据异常，请联系开发人员");
    return Promise.reject(res);
  },
  error => {
    Loading.hide();
    // 异常处理
    // const {
    //   data: { status, message = "" }
    // } = error.response;

    const message = get(error, "response.data.message");
    if (message && message.includes("凭证")) {
      //老java接口，登录过期判断
      gotoLogin();
    } else {
      Toast.error(message || "当前网络异常，请稍后刷新重试");
    }
    return Promise.reject(error);
  }
);

export default axios;

```


