<template>
    <div class="zip-upload-page">
<el-upload action="http://api.people.zhiruyi.cn/model/upload-tag-package-file" multiple :before-upload="zipbefore" :on-success="zipSuccess">
        <el-button size="small" type="primary">zip上传</el-button>
    </el-upload>
    </div>
</template>

<script>
import JSZip from 'jszip';
    export default {
        methods:{
            zipSuccess: function (response,file, list) {
            console.log('upload zip success:', response,file, list);
        },
        zipbefore:function (file)
        {
            let that = this;
            const isZip = new Promise((resolve,reject) => {
               JSZip.loadAsync(file).then(zip => {
                   async function valid()
                   {
                       //目录与文件分开处理，考虑到实际应用中，会有单独要处理的逻辑;
                       let folder = await that.folderInZip(zip).then().catch(e=>{});
                       let files = await that.fileInZip(zip).then().catch(e=>{console.log('e,e,e:>>',e)});
                       console.log('folder:>>',folder);
                       console.log('files:>>',files);
                       if(folder && files)
                       {
                           resolve(true);
                       }else
                       {
                            reject(false);
                       }
                   }
                   valid();
               }).catch(e => {
                  
                   reject(false);
               })
            }).then().catch(e => {reject(false)})
             console.log(':>>>>>>>>>>>',isZip);
            return false;
        },
        async fileInZip(zip)
        {
            let that = this;
            return new Promise((resolve,reject) => {
                for(let key in zip.files)
                {
                    if(!zip.files[key].dir)
                    {
                        let _name = zip.files[key].name;
                        async function asyncblob()
                        {
                           let blob = await new Promise((res,rej) => {
                               zip.file(_name).async('blob').then(blob => {res(blob)}).catch(e => {rej(false)})
                           })
                          let blobIsTodo = await that.blobTodo(blob,_name).then(res => {resolve(true)}).catch(e => {
                              reject(false);
                              });
                            console.log('blob is todo:>>',blobIsTodo);
                        }
                        
                        asyncblob();
                    }
                }
            })
        },
        async blobTodo(blob,name)
        {
            let that = this;
            return new Promise((resolve,reject) => {
                async function asyncTodo()
                {
                    let todo = await that.todoAsblob(blob,name).then(res => {
                        resolve(true);
                        }).catch(e => {
                            reject(false)});
                        
                };
                asyncTodo();
                
            })
        },
        //例：验证zip里，是否是图片
        async todoAsblob(blob,name)
        {
            return new Promise((resolve,reject) => {
                let image = new Image();
                image.onload = function(e)
                {
                    if(/\.(png|jpg|gif|jpeg)$/.test(name))
                    {
                        resolve(true);
                    }else
                    {
                        reject(false);
                    }
                };
                image.onerror = function(e)
                {
                    reject(false);
                };
               
                image.src=window.URL.createObjectURL(blob);
            })
        },
        async folderInZip(zip)
        {
            return new Promise((resolve,reject) => {
                let dir = [];
                for(let key in zip.files)
                {
                    let _name = zip.files[key].name;
                    if(zip.files[key].dir)
                    {
                        dir.push(_name);
                    }
                    resolve(dir);
                }
            })
        }
        }
    }
</script>

<style lang="scss">

</style>