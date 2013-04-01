描述：
<p>1.须要JQUERY版本：1.3.*或以上
2.整数支持到千亿，小数支持到厘
3.仅对中文进行支持</p>
配置项:
<p>
needinitdisplay:false/true,初始化时如果有值是否同步显示到指定的域，默认为false,当为TRUE，且绑定的域有值，指定的目标域存在时，将目标域显示出转换后的数据
forcefit:true/false,是否强制用户输入正确的金额格式,默认为真
values：["零", "壹", "贰", "叁", "肆", "伍", "陆", "柒", "捌", "玖"],用户可以自己定制大写内容
targets：[{用户必须指定目标域，
		targetid:'',目标域的ID，不包括#
		type:'',目标类型：，div,input,textarea
		displaymod:''显示模式：大写:words,千分制:thousands
	},{
		
	}

	],

</p>


用法：
引入JQUERY最新版本：
<pre lang='html'>
<script src="http://www.google.com/jsapi"></script>
<script>
  // Load jQuery
  google.load("jquery", "1.3.2");
</script>
</pre>
引入插件：
<pre lang='html'>
<script type='text/javascript' src='../jQuery.amountinwords.js' charset='gb2312'></script>
</pre>

例子：
<p>1.强制输入正确的金额格式：xxxxxxxxxxxxxx.xxx(x代表数字)</p>

<pre lang='javascript'>
$("#id").amountinwords({
	targets:[{
			targetid:'wordsresult',
			type:'div',
			displaymode:'words'
		}],
	forcefit:true
});
</pre>
<p>2.金额大写显示：</p>
<pre lang='javascript'>
$("#id").amountinwords({
	targets:[{
			targetid:'wordsresult',
			type:'div',
			displaymode:'words'
		}]
});
</pre>

<p>2.千分制形式显示金额：</p>
<pre lang='javascript'>
$("#id").amountinwords({
	targets:[{
			targetid:'wordsresult',
			type:'div',
			displaymode:'words'
		}]
});
</pre>
<p>4.多目标域显示结果:</p>
<pre lang='javascript'>
$("#id").amountinwords({
	targets:[{
			targetid:'wordsresult',
			type:'div',
			displaymode:'words'
		},{
			targetid:'thousandsresult',
			type:'input',
			displaymode:'thousands'
		}]
	forcefit:true,
	needinitdisplay:true,
	
});
</pre>
HTML部分代码：
<pre lang='html'>
<input type="text" name="orignamounts" value="32344450.03" id="orignamounts" />
<input type="text" name="thousandsresult" value="" id="thousandsresult" />
<div id="wordsresult"></div>
</pre>
注意：
<p>1.目前只支持到12位数字，即千亿;正打算开发支持更大的数据量</p>
<p>2.仅支持中文金额</p>
插件代码：
<pre lang='javascript' >
/***
*
*须要JQUERY版本：1.3.*或以上
*
*
*
*
*
*用法：
*引入插件：<script type='text/javascript' src='../jQuery.amountinwords.js' charset='gb2312'></script>
*needinitdisplay:false/true,初始化时如果有值是否同步显示到指定的域，默认为false,当为TRUE，且绑定的域有值，指定的目标域存在时，将目标域显示出转换后的数据
*forcefit:true/false,是否强制用户输入正确的金额格式,默认为真
*values：["零", "壹", "贰", "叁", "肆", "伍", "陆", "柒", "捌", "玖"],用户可以自己定制大写内容
*targets：[{用户必须指定目标域，
*		targetid:'',目标域的ID，不包括#
*		type:'',目标类型：，div,input,textarea
*		displaymod:''显示模式：大写:words,千分制:thousands
*	},{
*		
*	}
*
*	],
*
*
*例子：
*
*$("#id").amountinwords({
*	targets:[{
*			targetid:'wordsresult',
*			type:'div',
*			displaymode:'words'
*		},{
*			targetid:'thousandsresult',
*			type:'input',
*			displaymode:'thousands'
*		}]
*	forcefit:true,
*	needinitdisplay:true,
*	
*});
*HTML部分代码：
*<input type="text" name="orignamounts" value="32344450.03" id="orignamounts" />
*<input type="text" name="thousandsresult" value="" id="thousandsresult" />
*<div id="wordsresult"></div>
*
*注意：
*目前只支持到12位数字，即千亿;正打算开发支持更大的数据量
*
*
*
*
*
*
*
*
*
*
*
*
*
*
****/

(function($) {
	$.extend($.fn, {
		amountinwords: function(config) {
			config = $.extend({},
			{
				needinitdisplay: false,
				forcefit: true,
				digits: ["", "拾", "佰", "仟"],
				values: ["零", "壹", "贰", "叁", "肆", "伍", "陆", "柒", "捌", "玖"],
				adigits: ["圆", "万", "亿"],
				mindigits: ["厘", "分", "角"],
				targets: '' //转换后显示到指定的域，必须为INPUT,TEXTAREA等。传进来的对像必须含有targetid,type(div,input,textarea),displaymode(words,thousands)
			},
			config);
			if (config.needinitdisplay && !isNaN($(this).val()) && ($(this).val())) {
				displayresult($(this).val());
			}
			function displayresult(amount) {
				var resultcaches = new Array();
				$(config.targets).each(function(i, n) {
					if (n.displaymode == 'words') {
						if (!resultcaches['words']) {
							var _words = getamountinwords(amount);
							resultcaches['words'] = _words;
						}
						if (n.type == 'div') {
							$('#' + n.targetid).text(resultcaches['words']);
						} else if (n.type == 'input' || n.type == 'textarea') {
							$('#' + n.targetid).val(resultcaches['words']);
						}
					} else if (n.displaymode == 'thousands') {
						if (!resultcaches['thousands']) {
							var _thousands = getamountinthousands(amount);
							resultcaches['thousands'] = _thousands;
						}
						if (n.type == 'div') {
							$('#' + n.targetid).text(resultcaches['thousands']);
						} else if (n.type == 'input' || n.type == 'textarea') {
							$('#' + n.targetid).val(resultcaches['thousands']);
						}
					}
				});
			}
			function getamountinthousands(amount) {
				var txt = amount.split(".");
				while (/\d{4}(,|$)/.test(txt[0])) txt[0] = txt[0].replace(/(\d)(\d{3}(,|$))/, "$1,$2");
				var _result = txt[0] + (txt.length > 1 ? "." + txt[1] : "");
				return _result;
			}
			$(this).keyup(function() {
				var amount = this.value;
				if (config.forcefit) {
					amount = amount.replace(/[^\d\.]/g, "").replace(/(\.\d{3}).+$/, "$1");
					this.value = amount;
				}
				displayresult(amount);

			});
			function getamountinwords(amount) {
				if (isNaN(amount)) return "错误的数据格式";
				var number = Math.round(amount * 1000) / 1000;
				number = number.toString(10).split('.');
				var integer = number[0];
				var len = integer.length;
				if (len > 12) return "数值超出范围！支持的最大数值为 999999999999.99";

				var num = integer.split('');
				var dsl = num.length - 1;
				var _result = "";
				var _vi = 0;
				for (var i = 0; i <= (dsl > 12 ? 12 : dsl); i += 4) {

					var _t = integer.slice( - i - 4, len - i);
					var _ts = _t.split('');
					var _tslength = _ts.length - 1;
					var _tresult = '';
					for (var _i = 0; _i <= _tslength; _i++) {
						_tresult += config.values[parseInt(_ts[_i])] + (_ts[_i] != 0 ? config.digits[_tslength - _i] : '');
					}
					_tresult = _tresult.replace(/零+$/, "").replace(/零{2,}/, "零");

					_result = _tresult + ((_t != "0000" || _vi == 0) ? config.adigits[_vi] : "") + _result;
					_vi++;
				}

				//小数点后面处理
				if (number.length == 2) {
					var cok = number[1].split('');
					var _coklength = cok.length - 1;
					var _tresult = '';
					for (var i = 0; i <= _coklength; i++) {
						_tresult += config.values[parseInt(cok[i])] + (cok[i] != 0 ? config.mindigits[config.mindigits.length - 1 - i] : '');
					}
					_tresult = _tresult.replace(/零{2,}/, "零");
					_result += _tresult;
				}

				return _result;
			}
			return {
				init: function(obj) {
					if (isNaN($(this).val()) && ($(this).val() != '')) {
						alert(this.value);
						displayresult($(this).val());
					}
				}
			}
		}
	});
})(jQuery);

</pre>