<template lang="html">
       <div class="main">
         <mt-header title="首页"></mt-header>
         <router-view></router-view>
       </div>


</template>

<script>
import { MidPost } from '../assets/MidPost.js'
import all from '../../node_modules/vue-cookie/build/vue-cookie'
export default {
   data(){
     return {
  configData: {
    url: 'https://www.51kangduo.com/midserver.asq',
    requestJson: "{'openId':'1'}",
    Aeskey: '17C133AE614211DC',
    postData: {
      service: 'weixin.checkUser',
      partner: 'WYFRONTTEST',
      hospitalCode: '000004',
      dataFormat: 'JSON',
      inputCharset: 'utf-8',
      debug: true,
      requestEncrypted: '+l946Wek1izYa++D9OBmpw==',
      signType: 'md5',
      sign: '192C2382A2BFA47DCB52B961574CB525'
    }
  }
}
   },
  created () {
    //获取cookie
  function getCookie(cname) {
    var name = cname + "=";
    var ca = document.cookie.split(';');
    for (var i = 0; i < ca.length; i++) {
      var c = ca[i];
      while (c.charAt(0) == ' ') c = c.substring(1);
      if (c.indexOf(name) != -1) return c.substring(name.length, c.length);
    }
    return "";
  }

  function get(name) {

    var v = window.document.cookie.match('(^|;) ?' + name + '=([^;]*)(;|$)');

    return v ? v[2] : null;

    }

    //aes解密
    var that = this;

    function Decrypt(word) {
      var key = CryptoJS.enc.Utf8.parse(that.configData.Aeskey);
      var decrypt = CryptoJS.AES.decrypt(word, key, {
        mode: CryptoJS.mode.ECB,
        padding: CryptoJS.pad.Pkcs7
      });
      return CryptoJS.enc.Utf8.stringify(decrypt).toString();
    };

    var openid = Decrypt(getCookie('OpenId'));

    console.log("解密后"+openid);

    var requestJson = null;
    if (openid != '') {
    requestJson ={openId:openid}
    //console.log(requestJson)
    } else
    {
    requestJson = {openId:'test'}
    }

    console.log(JSON.stringify(requestJson))

    var that = this;
    MidPost.postMidserver({success:function(data){
      console.log(data.ifNew)
      if(data.ifNew && data.ifNew == 1){
        console.log('router register')
      }else{
        that.$router.push('/Register')
      }
    },
    service:"weixin.checkUser",
    requestjson: JSON.stringify(requestJson)
    })
}
}
</script>

<style lang="css">



</style>
