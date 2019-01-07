<template>
  <div id="app">
    <div><input v-model="userName" /></div>
    <img width="100%" :src="src" />
  </div>
</template>
<script>
import * as base64 from "qiniu-js/src/base64";
// eslint-disable-next-line
const UrlSafeBase64 = {
  encodeToString: function(content) {
    return base64.urlSafeBase64Encode(content);
  }
};

function getUserNameWidth(userName) {
  let chineseCount = 0; //汉字数量
  let uppercaseCount = 0; //大写字母数量
  let otherCount = 0; //其他数量
  let chars = userName.split(""); // toCharArray()
  chars.forEach(char => {
    if (char >= "A" && char <= "Z") {
      uppercaseCount++;
    } else if (/[\u4E00-\u9FA5]/.exec(char)) {
      chineseCount++;
    } else {
      otherCount++;
    }
  });

  return chineseCount * 9.5 + otherCount * 5 + uppercaseCount * 6.5;
}

const LOGO_WHITE = "https://assets.piaoniu.com/logo/logo_white.png";
export default {
  data() {
    return {
      width: 375,
      bottom: 6,
      right: 8,
      zoom: 2,
      userName: "我叫张家坤",
      imageUrlBase64: UrlSafeBase64.encodeToString(LOGO_WHITE),
      atBase64: UrlSafeBase64.encodeToString("@")
    };
  },
  computed: {
    userNameBase64() {
      let userName = this.userName;
      return UrlSafeBase64.encodeToString(userName);
    },
    src() {
      let img =
        "https://img.piaoniu.com//2788d1cbc95aa930eef39066184b32070505d091.jpg";
      return img + "?" + this.ops;
    },
    ops() {
      let {
        width,
        zoom,
        userNameBase64,
        right,
        bottom,
        atBase64,
        userName,
        imageUrlBase64
      } = this;
      let ops =
        "imageMogr2/" +
        "auto-orient/" + // 转正
        "thumbnail/" +
        width * zoom +
        "x/" + // 最大宽度
        "format/jpg/" + // 输出为jpg
        "blur/1x0/" + // 仅仅宽度缩放
        "quality/100" + // 质量75
        "|watermark/3" +
        // 文字
        "/text/" +
        userNameBase64 +
        "/font/5b6u6L2v6ZuF6buR/fontsize/380/fill/I0ZGRkZGRg==/dissolve/100/gravity/SouthEast" + // 字体颜色灯信息
        "/dx/" +
        right * zoom + // 右边
        "/dy/" +
        bottom * zoom + // 底边
        // @
        "/text/" +
        atBase64 +
        "/font/5b6u6L2v6ZuF6buR/fontsize/440/fill/I0ZGRkZGRg==/dissolve/100/gravity/SouthEast" + // 字体颜色灯信息
        "/dx/" +
        Math.round((right + getUserNameWidth(userName)) * zoom) + // 右边
        "/dy/" +
        (bottom - 1) * zoom + // 底边
        "/image" +
        "/" +
        imageUrlBase64 +
        "/dissolve/100/gravity/SouthEast" +
        "/dx/" +
        Math.round((getUserNameWidth(userName) + right + 12) * zoom) + // 右边还要算出文字 + @ 的宽度
        "/dy/" +
        (bottom + 1) * zoom;
      return ops;
    }
  }
};
</script>
<style lang="stylus">
body {
  margin: 0;
}
</style>
