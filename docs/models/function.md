## 原子封装
所有接口访问需要携带原子封装，在src/server/api.js文件中处理：
```js
/*** 在请求数据之前的处理 */
/*
 *@params type 请求的类型 post、get、delete、put
 *@params data 发送请求所携带的参数的集合
  */
let ajax_before = (type, data) => {
	if(typeof data === 'undefined') {
		data = {};
	}
//主要针对数据进行进行一些公共化的处理
	let temp_obj;
	temp_obj = {
		view_type: 'h5',
		cc: 'G1000',
		cv: 'FB3.1.1.00_A4.4',
		ua: 'WebKit',
		sign_time: '1520931947',
		sign_random: 'Y2zX0ZNi',
		imei: '1520931947',
		imsi: '222222222',
		sign_id:'10000',
	}
	for (let i in data) {
		temp_obj[i] = data[i];
	}
	temp_obj.sign_time = Math.ceil((new Date()).getTime() / 1000);
	temp_obj.sign_random = Math.ceil(Math.random() * 10000000);
	let sign_key = 'Fubang.119*(';
	if(process.env.ALONE === 'longnan'){		//独立部署陇南项目需要修改一下参数
		temp_obj.cv = 'LN3.1.1.00_A4.4';
        temp_obj.sign_id = '10010';
        sign_key = '3Qykmp3FYSbq&ug5';
    }
    else if(process.env.ALONE === 'zhongwei'){   //独立部署中卫项目需要修改一下参数
    	temp_obj.cv = 'ZW3.1.1.00_A4.4';
        temp_obj.sign_id='10020';
        sign_key='RZ2*KRpxLFEO%2FK';
    }
	let md5_before = '';
	for (let i in temp_obj) {
		md5_before += "&" + i + "=" + temp_obj[i];
	}
	//所有参数采用md5加密
	temp_obj.sign = md5.md5_32(`method=${type}&sign_key=${sign_key}${md5_before}`);
	return temp_obj;
}
```
## 请求接口（axios）
### 1.安装（axios）
```python
npm install --save axios 或 npm i -S axios
```
也可以通过cdn引入
```html
<script src="https://cdn.bootcss.com/axios/0.18.0/axios.min.js"></script>
```
### 2.src/server/api.js文件中引入
```js
import axios from 'axios'
```
设置axios
```js
// 全局的配置的默认值/defaults
export let axios_config = () => {
	axios.defaults.baseURL = baseUrl;
	axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
	axios.defaults.timeout = 60000;
}
axios_config();
```
### 3.使用
在src/server/api.js文件中使用 
#### 3-1.get请求
```js
/*
 *@params data 发送请求所携带的参数的集合
 */
export const owner_alarm_detail = (data) => {
	data = ajax_before('get', data);	//请求接口前做原子封装处理
	return axios.get('/fe/api_proxy/event/info/', {
		params: data
	}).then(result => {
		return getAjax(result);
	});
}

//接口暴露  owner_alarm_detail 供单个组件使用
```
#### 3-2.post请求
```js
/*
 *@params data 发送请求所携带的参数的集合{queryData}
 */
export const register = (data) => {
	let query = ajax_before('post', data.queryData);	//请求接口前做原子封装处理
	console.log(query)
	return axios.request({
		url: '/fe/api_proxy/user/register/',
		method: 'post',
		params: query,
		data: data.paramsData,
	}).then(result => {
		return getAjax(result);
	})
}

//接口暴露  register 供单个组件使用
```
#### 3-3.delete请求
```js
/*
 *@params data 发送请求所携带的参数的集合
 */
export const apply_delete = (data) => {
	data = ajax_before('delete', data);				//请求接口前做原子封装处理
	console.log(data)
	return axios.delete('/fe/api_proxy/bind/info/', {
		params: data
	}).then(result => {
		return getAjax(result);
	});
}

//接口暴露  apply_delete 供单个组件使用
```
#### 3-4.put请求
```js
/*
 *@params data 发送请求所携带的参数的集合{queryData}
 */
export const edit_base_info = (data) => {
	let query = ajax_before('put', data.queryData);		//请求接口前做原子封装处理
	console.log(query)
	return axios.request({
		url: '/fe/api_proxy/company_firestation/info/',
		method: 'put',
		params: query,
		data: data.paramsData,
	}).then(result => {
		return getAjax(result);
	})
}

//接口暴露  edit_base_info 供单个组件使用
```
## 请求结果处理
对请求结果进行判断，正常就返回，不正常就统一进行处理
```js
/*
 *@params result 发送请求接口响应返回的结果
 */
var getAjax = result => {
	return new Promise((resolve, reject) => {
		if((typeof result.data.code !== 'undefined' && result.data.code === 0) || (typeof result.data.status !== 'undefined' && result.data.status === 0)) {
			
			loading(false);		//关闭loading弹框
			if(result.data.data){
				resolve(result.data);		//返回响应结果
			}else{
				resolve();
			}
			
		} else {
		//对异常情况统一处理
			loading(false);
			if(result.data.code) {
				if(result.data.code == '9996') {
					phone_send({
						type: 'error',
						data: {
							code: '9996',
							msg: 'token过期'
						}
					});
				} else if(result.data.code == '7995') {
					app.toast('数据已存在')
				}else{
					app.toast(result.data.msg)
				}
			} else {
				if(result.data.status == '9996') {
					phone_send({
						type: 'error',
						data: {
							code: '9996',
							msg: 'token过期'
						}
					});
				}else if(result.data.status == '7995'){
					app.toast('数据已存在')
				}else{
					app.toast(result.data.msg)
				}
			}
			reject(result.data);
		}
	})
}
```
## 数据类型判断函数（封装）
>函数名称：typeOf

```js
/*
 *@params obj 要判断的数据
 */
const typeOf = (obj) => {
  const toString = Object.prototype.toString;
  const map = {
    '[object Boolean]': 'boolean',
    '[object Number]': 'number',
    '[object String]': 'string',
    '[object Function]': 'function',
    '[object Array]': 'array',
    '[object Date]': 'date',
    '[object RegExp]': 'regExp',
    '[object Undefined]': 'undefined',
    '[object Null]': 'null',
    '[object Object]': 'object'
  };
  return map[toString.call(obj)];
}
```
## 字典表取值
>函数名称：getObjectValue

```js
/*
 *@params obj Object 要取字段所在的字典表
 *@params text String 要取的字段如 'a.b.c'
 */
export const getObjectValue = function(obj, text) {
	try {
		if((_typeOf(obj) === 'object' || _typeOf(obj) === 'array') && text) {
			let textArray = text.split('.');
			let get_value = function(obj, textArray) {
				let key = textArray.shift();
				if(key.length < 5 && parseInt(key)) {
					key = parseInt(key);
				}
				if(typeof obj[key] == 'undefined' || obj[key] == null) {
					return '';
				}
				if(textArray.length == 0) {
					return obj[key];
				}
				obj = obj[key];
				return get_value(obj, textArray);
			}
			return get_value(obj, textArray);
		}
		return '';
	} catch(error) {
		console.log(error);
	}
}
```
## 判断设备类型
>函数名称：get_deviceInfo

```js
export const get_deviceInfo = () => {
	var device = navigator.userAgent;
	if(device.match('Android')) {
		return 'android';
	} else if(device.match('iPhone')) {
		return 'iphone';
	} else {
		return 'pc';
	}
}
```
## 本地存储
>函数名称：setsessionStorage、getsessionStorage、removesessionStorage
### 设置储存的值（setsessionStorage）
>函数名称：setsessionStorage

```js
/*
 *@params keys String 给存值定义键名
 *@params value String/Object 要存的值
 */
export const setsessionStorage = (keys, value) => {
	if(sessionStorage) {
		var jsom = sessionStorage['jsom'];
		var mp = {};
		if(jsom && jsom != '') {
			mp = JSON.parse(jsom);
		}
		mp[keys] = value;
		jsom = JSON.stringify(mp);
		sessionStorage['jsom'] = jsom;
	}
	return '';
}
```
### 获取储存的值（getsessionStorage）
>函数名称：getsessionStorage

```js
/*
 *@params keys String 存值时定义键名
 */
export const getsessionStorage = (keys) => {
	if(sessionStorage && sessionStorage['jsom'] != undefined) {
		var jsom = sessionStorage['jsom'];
		if(jsom && jsom != '') {
			var mp = JSON.parse(jsom);
			if(mp[keys] && mp[keys] != '') {
				return mp[keys];
			} else {
				return "";
			}
		}
	} else {
		return "";
	}
}
```
### 删除储存的值（removesessionStorage）
>函数名称：removesessionStorage

```js
/*
 *@params keys String 存值时定义键名
 */
export const removesessionStorage = (keys) => {
	if(sessionStorage) {
		var jsom = sessionStorage['jsom'];
		if(jsom && jsom != '') {
			var mp = JSON.parse(jsom);
			var _mp = {};
			if(keys === undefined) {
				for(var i in mp) {
					if(i === '_key' || i === 'uname') {
						_mp[i] = mp[i];
					}
				}
			} else {
				for(var i in mp) {
					if(i !== keys) {
						_mp[i] = mp[i];
					}
				}
			}

			_mp = JSON.stringify(_mp);
			sessionStorage['jsom'] = _mp;
		}
		return '';
	}
}
```
## 回路部件号与楼号、单元号、房间号互转
### 回路部件号转楼号、单元号、房间号（getBuild_unit_room_num）
>函数名称：getBuild_unit_room_num

```js
/*
 *@params looper_num Number 回路号
 *@params component_num Number 部件号
 */
export const getBuild_unit_room_num = (looper_num,component_num)=>{
		let obj = {};
		obj.room_num = component_num;
		let looper_num10 = parseInt(looper_num).toString(16);
		looper_num10 = looper_num10.length==3? '0'+looper_num10:looper_num10;
		obj.unit_num = parseInt(looper_num10.substring(0,2),16);
		obj.building_num = parseInt(looper_num10.substring(2,4),16);
		return obj;		//返回结果，可得building_num、unit_num、room_num
};
```
### 楼号、单元号、房间号转回路部件号（getLooper_component）
>函数名称：getLooper_component

```js
/*
 *@params building_num Number 楼号
 *@params unit_num Number 单元号
 *@params room_num Number 房间号
 */
export const getLooper_component = (building_num,unit_num,room_num) =>{
		let obj = {};
		obj.component_num = parseInt(room_num);
		let building_num16 = parseInt(building_num).toString(16);
		let unit_num16 = parseInt(unit_num).toString(16);
		if(building_num16.length<2){
			building_num16 = '0'+building_num16;
		}
		if(unit_num16.length<2){
			unit_num16 = '0'+unit_num16;
		}
		let newStr = unit_num16+building_num16;
		obj.looper_num = parseInt(newStr,16);
		return obj;		//返回结果，looper_num、component_num
	};

```
## 时间格式处理
>函数名称：data_formatting

```js
/*
 *@params value String 需要转化的时间字符串：2019-01-16 10:39:20
 */
export const data_formatting = (value) => {
  let result = '';
  let now_date = new Date();
  let now_year = now_date.getFullYear();
  let now_month = now_date.getMonth();
  let now_ri = now_date.getDate();

  let yestoday = new Date(now_date.setTime(now_date.getTime() - 24 * 60 * 60 * 1000)).getDate();

  value = value.toString();
  let date = new Date(value.replace(/\-/g,'/'));
  let year = date.getFullYear();
  let month = date.getMonth();
  let value_date = date.getDate();
  let time = value.split(' ')[1];
  time=time.slice(0,5);
  if (year == now_year) {	//今年
    if (month == now_month && value_date == now_ri) {
      result = '今天 ' + time;	//如：今天 10:39
    } else if (month == now_month && value_date == yestoday) {
      result = '昨天 ' + time;	//如：昨天 10:39
    } else {
      result = value.slice(5, value.length-3);	//如：01-16 10:39
    }
  } else {//今年以前
    result = value.slice(0,value.length-3);     //如：2019-01-16 10:39
  }
  return result;
}
```
## 数组去重
>函数名称：uni

```js
/*
 *@params array Array 需要做去重处理的数组
 */
export const uni = function(array){
	let obj ={};
	let arr = [];
	for(let i=0;i<array.length;i++){
		let str = array[i]+typeof(array[i]);
		if(!obj[str]){
			obj[str] = true;
			arr.push(array[i])
		}
	}
	return arr;	//返回去重后的数组
}
```
## 对象深克隆（deepCopy）
>函数名称：deepCopy

```js
/*
 *@params array Array 需要做去重处理的数组
 */
export const deepCopy = function(obj)=>{
	let strObj = JSON.stringify(obj);
	return JSON.parse(strObj);
};
```
## 指令
### v-value
>监听元素data-placeholder值变化并改变字体颜色

```js
/*
 *value  指令名称
 */
Vue.directive('value', {
  // 当被绑定的元素插入到 DOM 中时
  inserted: function (el) {
  	let innerHTML = el.innerHTML;
    if(el.dataset.placeholder!=innerHTML){
    	el.style.color='#333333';
    }else{
    	el.style.color='#999999';
    }
  },
  componentUpdated:function (el) {
    // 聚焦元素
  	let innerHTML = el.innerHTML;
    if(el.dataset.placeholder!=innerHTML){
    	el.style.color='#333333';
    }else{
    	el.style.color='#999999';
    }
  },
})
```
value指令的使用：<br/>
```html
<div v-value data-placeholder="请选择关联用户信息传输装置">{{text}}</div>
```
## 过滤器
### NullStr
>过滤返回值为空、0、0.0、-1时同意默认为--

```js
/*
 *NullStr  过滤器名称
 */
Vue.filter('NullStr', function (str) {
 if((str+'' === '')||(str+'' === '0')||(str+'' === '0.0')||(str == -1)){
		return '--';
	}else{
		return str;
	}
})
```




