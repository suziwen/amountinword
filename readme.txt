������
<p>1.��ҪJQUERY�汾��1.3.*������
2.����֧�ֵ�ǧ�ڣ�С��֧�ֵ���
3.�������Ľ���֧��</p>
������:
<p>
needinitdisplay:false/true,��ʼ��ʱ�����ֵ�Ƿ�ͬ����ʾ��ָ������Ĭ��Ϊfalse,��ΪTRUE���Ұ󶨵�����ֵ��ָ����Ŀ�������ʱ����Ŀ������ʾ��ת���������
forcefit:true/false,�Ƿ�ǿ���û�������ȷ�Ľ���ʽ,Ĭ��Ϊ��
values��["��", "Ҽ", "��", "��", "��", "��", "½", "��", "��", "��"],�û������Լ����ƴ�д����
targets��[{�û�����ָ��Ŀ����
		targetid:'',Ŀ�����ID��������#
		type:'',Ŀ�����ͣ���div,input,textarea
		displaymod:''��ʾģʽ����д:words,ǧ����:thousands
	},{
		
	}

	],

</p>


�÷���
����JQUERY���°汾��
<pre lang='html'>
<script src="http://www.google.com/jsapi"></script>
<script>
  // Load jQuery
  google.load("jquery", "1.3.2");
</script>
</pre>
��������
<pre lang='html'>
<script type='text/javascript' src='../jQuery.amountinwords.js' charset='gb2312'></script>
</pre>

���ӣ�
<p>1.ǿ��������ȷ�Ľ���ʽ��xxxxxxxxxxxxxx.xxx(x��������)</p>

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
<p>2.����д��ʾ��</p>
<pre lang='javascript'>
$("#id").amountinwords({
	targets:[{
			targetid:'wordsresult',
			type:'div',
			displaymode:'words'
		}]
});
</pre>

<p>2.ǧ������ʽ��ʾ��</p>
<pre lang='javascript'>
$("#id").amountinwords({
	targets:[{
			targetid:'wordsresult',
			type:'div',
			displaymode:'words'
		}]
});
</pre>
<p>4.��Ŀ������ʾ���:</p>
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
HTML���ִ��룺
<pre lang='html'>
<input type="text" name="orignamounts" value="32344450.03" id="orignamounts" />
<input type="text" name="thousandsresult" value="" id="thousandsresult" />
<div id="wordsresult"></div>
</pre>
ע�⣺
<p>1.Ŀǰֻ֧�ֵ�12λ���֣���ǧ��;�����㿪��֧�ָ����������</p>
<p>2.��֧�����Ľ��</p>
������룺
<pre lang='javascript' >
/***
*
*��ҪJQUERY�汾��1.3.*������
*
*
*
*
*
*�÷���
*��������<script type='text/javascript' src='../jQuery.amountinwords.js' charset='gb2312'></script>
*needinitdisplay:false/true,��ʼ��ʱ�����ֵ�Ƿ�ͬ����ʾ��ָ������Ĭ��Ϊfalse,��ΪTRUE���Ұ󶨵�����ֵ��ָ����Ŀ�������ʱ����Ŀ������ʾ��ת���������
*forcefit:true/false,�Ƿ�ǿ���û�������ȷ�Ľ���ʽ,Ĭ��Ϊ��
*values��["��", "Ҽ", "��", "��", "��", "��", "½", "��", "��", "��"],�û������Լ����ƴ�д����
*targets��[{�û�����ָ��Ŀ����
*		targetid:'',Ŀ�����ID��������#
*		type:'',Ŀ�����ͣ���div,input,textarea
*		displaymod:''��ʾģʽ����д:words,ǧ����:thousands
*	},{
*		
*	}
*
*	],
*
*
*���ӣ�
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
*HTML���ִ��룺
*<input type="text" name="orignamounts" value="32344450.03" id="orignamounts" />
*<input type="text" name="thousandsresult" value="" id="thousandsresult" />
*<div id="wordsresult"></div>
*
*ע�⣺
*Ŀǰֻ֧�ֵ�12λ���֣���ǧ��;�����㿪��֧�ָ����������
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
				digits: ["", "ʰ", "��", "Ǫ"],
				values: ["��", "Ҽ", "��", "��", "��", "��", "½", "��", "��", "��"],
				adigits: ["Բ", "��", "��"],
				mindigits: ["��", "��", "��"],
				targets: '' //ת������ʾ��ָ�����򣬱���ΪINPUT,TEXTAREA�ȡ��������Ķ�����뺬��targetid,type(div,input,textarea),displaymode(words,thousands)
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
				if (isNaN(amount)) return "��������ݸ�ʽ";
				var number = Math.round(amount * 1000) / 1000;
				number = number.toString(10).split('.');
				var integer = number[0];
				var len = integer.length;
				if (len > 12) return "��ֵ������Χ��֧�ֵ������ֵΪ 999999999999.99";

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
					_tresult = _tresult.replace(/��+$/, "").replace(/��{2,}/, "��");

					_result = _tresult + ((_t != "0000" || _vi == 0) ? config.adigits[_vi] : "") + _result;
					_vi++;
				}

				//С������洦��
				if (number.length == 2) {
					var cok = number[1].split('');
					var _coklength = cok.length - 1;
					var _tresult = '';
					for (var i = 0; i <= _coklength; i++) {
						_tresult += config.values[parseInt(cok[i])] + (cok[i] != 0 ? config.mindigits[config.mindigits.length - 1 - i] : '');
					}
					_tresult = _tresult.replace(/��{2,}/, "��");
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