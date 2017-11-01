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



> 数字转中文

	function transformNum(val) {
	    val = parseInt(val).toString();
	    var danwei = ['', '十', '百', '千', '万', '十', '百', '千', '亿'];
	    var num = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九'];
	    var l = val.length;
	    var a = new Array(l);
	    var b = new Array(l);
	    var result = '';
	    for (var i = 0; i < l; i++) {
	      a[i] = val.substr(i, 1);
	      b[i] = num[a[i]];
	      result += b[i] + danwei[l - i - 1];
	    }
	    if (result.lastIndexOf('零') === result.length - 1) {
	      result = result.substring(0, result.lastIndexOf('零'));
	    }
	    if (result.indexOf('一十') === 0) {
	      result = result.substring(1);
	    }
	    return '' + result;
	}

> 返回字符串长度，汉字计数为2 包括汉字符号

	function strLength(str) {
		var a = 0;
		for(var i = 0; i < str.length; i++) {
			if (str.charCodeAt(i)>255) {
				a += 2;
			} else {
				a++;
			}
		}
		return a;
	}

> 获取URL中的参数

	function GetQueryStringRegExp(name,url) {
		var reg = new RegExp("(^|\?|&)" + name + "=([^&]*)(\s|&|$)", "i");
		if (reg.test(url)) return decodeURIComponent(RegExp.$2.replace(/+/g, " "));
		return "";
	}

> 获取类url字符串中参数的集合
	
	function getParams(str) {
		str = str ? str : window.location.href;
		str = encodeURIComponent(decodeURIComponent(str));
		var t = null, reg = /[?&]([^=]*)=([^&?]*)/g;
		var params = {};
		if (str.lenght < 1) {
			return params;
		}
		while(t = reg.exec(str)) {
			params[decodeURIComponent(t[1])] = decodeURIComponent(t[2]);
		}
		return params;
	}

> 获取浏览器前缀
	
	function getPrefix() {
		var vendorPrefixes = ['Moz', 'Webkit', 'Khtml', 'O', 'ms'],
		len = vendorPrefixes.lenht,
		vendor = '';
		while(len--) {
			if ((vendorPrefixes[len] + 'Transform') in document.body.style) {
				vendor = '-' + vendorPrefixes[len].toLowerCase() + '-';
			}
		}
		return vendor;
	}

> 探测 transitionEnd 事件
	
	function getTransitionEnd() {
	  var t,
	    el = document.createElement('fakeelement'),
	    transitions = {
	      'transition':'transitionend',
	      'OTransition':'oTransitionEnd',
	      'MSTransition':'transitionend',
	      'MozTransition':'transitionend',
	      'WebkitTransition':'webkitTransitionEnd'
	    };

	  for (t in transitions) {
	    if (el.style[t] !== undefined) {
	      return transitions[t];
	    }
	  }
	}

> requestAnimationFrame 事件兼容

	window.requestAnim = (function() {
	  return window.requestAnimationFrame ||
	    window.webkitRequestAnimationFrame ||
	    window.mozRequestAnimationFrame ||
	    window.oRequestAnimationFrame ||
	    window.msRequestAnimationFrame ||
	    function(callback) {
	      window.setTimeout(callback, 1000 / 60);
	    };

	})();

> 函数节流（throttle） 与  函数去抖（debounce）

 当不断触发页面重绘或者资源不断加载的事件等行为，可能会导致UI停顿甚至浏览器崩溃

  1. window对象的resize、scroll事件
  
  2. 拖拽时的mousemove事件
  
  3. 射击游戏中的mousedown、keydown事件
  
  4. 文字输入、自动完成的keyup事件
  
  实际上对于window的resize事件，实际需求大多为停止改变大小n毫秒后执行后续处理；而其他事件大多的需求是以一定的频率执行后续处理。针对这两种需求就出现了debounce和throttle两种解决办法。

  预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期
	var throttle = function(fn, delay) {
	  var now, lastExec, timer, context, args; //eslint-disable-line

	  var execute = function() {
	    fn.apply(context, args);
	    lastExec = now;
	  };

	  return function() {
	    context = this;
	    args = arguments;
	    // console.log(context.id);
	    now = Date.now();

	    if (timer) {
	      clearTimeout(timer);
	      timer = null;
	    }
	    // debugger; // eslint-disable-line
	    if (lastExec) {
	      var diff = delay - (now - lastExec);
	      // console.log(diff);
	      if (diff < 0) {
	        execute();
	      } else {
	        timer = setTimeout(() => {
	          execute();
	        }, diff);
	      }
	    } else {
	      execute();
	    }
	  };
	};
	
  调用动作delay毫秒后，才会执行该动作，若在这n毫秒内又调用此动作则将重新计算执行时间。

	var debounce = function(fn, delay){
	  var last;
	  return function(){
	    var ctx = this, args = arguments;
	    clearTimeout(last);
	    last = setTimeout(function(){
	      fn.apply(ctx, args);
	    }, delay);
	  }
	}


JS魔法堂：函数节流（throttle）与函数去抖（debounce） 
http://www.cnblogs.com/fsjohnhuang/p/4147810.html  


