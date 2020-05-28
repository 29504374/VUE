<template>
<div class="image-upload-page">
    
    <el-upload action="/upload-image/" multiple :before-upload="imagebefore" :on-success="imageSuccess">
        <el-button size="small" type="primary">图片上传</el-button>
    </el-upload>
</div>
</template>

<script>
export default {
    methods: {
        imageSuccess: function (file, list) {
            console.log('upload success:', file, list);
        },
        imagebefore: function (file) {
            let that = this;
            let isPicture = new Promise((resolve, reject) => {
                async function valid() {
                    let binary = await that.fileTobinary(file).then().catch(e => {});
                    let picture = await that.binaryToImage(binary,file.name).then().catch(e =>{});
                   if(picture)
                   {
                       resolve(true);
                   }else
                   {
                       reject(false);
                   }
                }
                valid();
            }).then().catch(e => {reject(false);})
            console.log('is picture:',isPicture);
            return isPicture;
        },
        async binaryToImage(binary,name)
        {
            return new Promise((resolve,reject) => {
                let image = new Image();
                image.onload = function(e)
                {
                    console.log('这里是对image对象操作......')
                    if(/\.(png|jpg|gif|jpeg)$/.test(name))
                    {
                        resolve(true);
                    }else
                    {
                        reject(false);
                    }
                }
                image.onerror = function(e)
                {
                     console.log('image error......')
                        reject(false);
                }
                    image.src = binary;
            })
        },
        async fileTobinary(file) {
            return new Promise((resolve, reject) => {
                let reader = new FileReader();
                let valid = false;
                reader.onload = function (e) {
                    resolve(e.target.result)
                }
                reader.onerror = function(e)
                {
                    reject(false)
                }
                reader.readAsDataURL(file);
            })
        }
    }
}
</script>

<style lang="scss">

</style>
