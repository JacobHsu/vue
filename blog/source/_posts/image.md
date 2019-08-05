---
title: Image 
---

## Image

KycForm.vue 父  

```html
<kyc-image v-for="img in imgArr"
    :key="img.pos"
    :text="img.text"
    :imgSrc.sync="form.imageList[img.pos]"
    :id="`img_${img.pos}`"
    class="mt-3">
</kyc-image>
```

KycImage.vue 子  

```html
<input accept="image/jpeg, image/png"
    type="file"
    :id="(id)"
    @change="handleLoad"
    :class="[$style.idimg__input]" />
```

```js
  // props 父組件 傳遞資料給 子組件
  props: {
    imgSrc: {
      type: String,
      default: null
    },
```


```js
  methods: {
    handleLoad(evt) {
      this.isLoading = true
      const imgFile = evt.target.files[0]
      // console.log(imgFile, 'orig image')
      if (!imgFile) {
        this.applyStyle(null)
        return
      } else {
        this.applyStyle(this.imgSrc) // this.imgSrc 從props來 
        return
      }
    },
    /**
     * 將圖片套用在CSS
     */
    applyStyle(src) {
      const imgStyle = this.$refs[this.id].style // $refs 取得 DOM Element 的資訊
      imgStyle.backgroundImage = ''
      if (src) {
        imgStyle.backgroundImage = `url(${src})`
        this.$emit('update:imgSrc', src) // 子組件 傳遞資料給 父組件 
        imgStyle.backgroundRepeat = 'no-repeat'
        imgStyle.backgroundPosition = 'center'
        imgStyle.backgroundSize = 'contain'
      } else {
        this.$emit('update:imgSrc', '')
      }
      this.isLoading = false
    },

```
