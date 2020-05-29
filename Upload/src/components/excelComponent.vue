<template>
<div class="excel-upload-page">
    <el-upload action="/upload-excel/" multiple :before-upload="excelbefore" :on-success="excelSuccess">
        <el-button size="small" type="primary">excel上传</el-button>
    </el-upload>
</div>
</template>

<script>
import XLSX from 'xlsx';
export default {
    methods: {
        excelSuccess: function (response, file, list) {
            console.log('upload excel success:', response, file, list);
        },
        excelbefore: function (file) {
            let that = this;
            const isXLSX = new Promise((resolve, reject) => {
                async function vaild() {
                    let binary = await that.fileTobinary(file).then().catch(e => {});
                    let json = await that.excelToJson(binary).then().catch(e => {});
                    if(json)
                    {
                        resolve(true);
                    }else
                    {
                        reject(false);
                    }
                }
                vaild();
            }).then().catch(e =>{reject(false)});
            return isXLSX;
        },
        async excelToJson(binary) {
            return new Promise((resolve, reject) => {

                try {
                    let xlsx = XLSX.read(binary, {
                        type: 'binary'
                    });
                    let result = [];
                    if (xlsx.Props.Application == "Microsoft Excel") {
                        xlsx.SheetNames.forEach(sheetName => {
                            result.push({
                                sheetName: sheetName,
                                sheet: XLSX.utils.sheet_to_json(xlsx.Sheets[sheetName])
                            })
                        })
                        resolve(result);
                    } else {
                        reject(false);
                    }
                } catch (e) {
                    reject(false);
                }

            })
        },
        async fileTobinary(file) {
            return new Promise((resolve, reject) => {
                let reader = new FileReader();
                reader.onload = function (e) {
                    resolve(e.target.result);
                };
                reader.onerror = function (e) {
                    reject(false);
                };
                reader.readAsBinaryString(file);
            })
        }
    }
}
</script>

<style lang="scss">

</style>
