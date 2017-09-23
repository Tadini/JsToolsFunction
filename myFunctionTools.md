# 工具函数集

## 检测浏览器是否支持css属性

> 检测单一属性

     function testProperty(property) {
		var body = document.body;
		if (property in body.style) {
			return true;		
		}
		return false;
    }
